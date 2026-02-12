# NRP Training - Intermediate Track

**February 2026 Training**

This notebook covers the Intermediate Track topics for the National Research Platform (NRP) training:
1. Using NRP's LLMs
2. Using JupyterHub
3. Using Coder
4. Deploying JupyterHub
5. Building in NRP's Gitlab

---

## Prerequisites

Before starting, ensure you have:
- ✅ Completed the Beginner Track or have equivalent Kubernetes knowledge
- ✅ NRP access in Authentik
- ✅ Namespace admin privileges (required for Section 4: Deploying JupyterHub)
- ✅ kubectl installed and configured
- ✅ Helm installed (for JupyterHub deployment)

---

## Resources

- [NRP Portal](https://nrp.ai/)
- [NRP Documentation](https://docs.nrp.ai/)
- [JupyterHub Documentation](https://jupyterhub.readthedocs.io/)
- [Coder Documentation](https://coder.com/docs)


# Part 1: Using NRP-Managed LLMs

## Overview

The NRP provides several hosted open-weights Large Language Models (LLMs) for either API access or use with hosted chat interfaces. These models are managed by NRP and available to users who are members of a group with LLM flag enabled.

## Key Features

- **Multiple Models**: Access to various state-of-the-art LLMs including Qwen, GLM, Kimi, GPT-OSS, and more
- **Chat Interfaces**: Web-based chat interfaces similar to ChatGPT
- **API Access**: OpenAI-compatible API for programmatic access
- **Tool Calling**: Many models support function calling and tool use
- **Multimodal Support**: Some models support vision and video inputs

## Chat Interfaces

### Open WebUI

NRP provides an Open WebUI interface for chatting with hosted LLMs:
- **URL**: Available through the NRP Portal
- **Features**: Feature-filled chat interface for all NRP-hosted models
- **Access**: Similar to ChatGPT interface

### LibreChat

NRP also provides LibreChat as an alternative chat interface:
- **URL**: Available through the NRP Portal
- **Features**: Simple chat interface for all NRP-hosted models

### Chatbox

You can also use the standalone Chatbox app:
- **Web Interface**: Available through the NRP Portal
- **Standalone App**: Can be installed on your local machine
- **Configuration**: Generate config from the LLM token generation page

## API Access

To access NRP LLMs programmatically, you need:

1. **Group Membership**: Be a member of a group with LLM flag enabled (check on the [namespaces page](https://nrp.ai/namespaces/))
2. **API Token**: Generate a token from the NRP Portal (LLM Tokens section)
3. **API Endpoint**: `https://ellm.nrp-nautilus.io/v1`

### Example: List Available Models

```bash
curl -H "Authorization: Bearer <your_token>" https://ellm.nrp-nautilus.io/v1/models
```

### Example: Curl + Bash

```bash
curl -H "Authorization: Bearer <TOKEN>" -X POST "https://ellm.nrp-nautilus.io/v1/chat/completions" \
-H "Content-Type: application/json" \
-d '{
    "model": "gpt-oss",
    "messages": [
      {"role": "user", "content": "How do I check if a Python object is an instance of a class?"}
    ]
  }'
```

### Example: Python Client

```python
import os
from openai import OpenAI

client = OpenAI(
    # This is the default and can be omitted
    api_key = os.environ.get("OPENAI_API_KEY"),
    base_url = "https://ellm.nrp-nautilus.io/v1"
)

completion = client.chat.completions.create(
    model="gpt-oss",
    messages=[
        {"role": "system", "content": "Talk like a pirate."},
        {
            "role": "user",
            "content": "How do I check if a Python object is an instance of a class?",
        },
    ],
)

print(completion.choices[0].message.content)
```

**Important Notes:**
- **Do NOT specify `max_tokens` or `max_output_tokens`** - leave it empty or set to less than half the context length
- **Context Windows**: Models have different context sizes (e.g., 131K, 262K tokens)
- **Model Status**: Check model status (main, evaluating, deprecated) before using

## Available Models

NRP provides various models with different capabilities:

- **qwen3**: Multimodal, 262K tokens, tool calling, frontier performance
- **gpt-oss**: 131K tokens, tool calling, agentic tasks
- **kimi**: 262K tokens, tool calling, coding performance
- **glm-4.7**: 202K tokens, tool calling, coding performance
- **glm-v**: Multimodal, 131K tokens, vision/video support
- **gemma3**: Multimodal, 131K tokens, tool calling
- **embed-mistral**: Embedding model (not for chat)

For the complete list, status, and capabilities, see the [NRP LLM documentation](https://nrp.ai/documentation/userdocs/ai/llm-managed/).

## Getting Started

1. **Check Access**: Verify you're in a group with LLM flag at [https://nrp.ai/namespaces/](https://nrp.ai/namespaces/)
2. **Get Token**: Generate an API token from the NRP Portal
3. **Try Chat Interface**: Use Open WebUI or LibreChat to test models
4. **Use API**: Integrate into your applications using the OpenAI-compatible API

For detailed documentation, examples, and model information, visit: [https://nrp.ai/documentation/userdocs/ai/llm-managed/](https://nrp.ai/documentation/userdocs/ai/llm-managed/)


# Part 2: Using JupyterHub

## Overview

JupyterHub is a multi-user server for Jupyter notebooks. It allows users to launch and use interactive development environments, making it ideal for researchers, students, and educators.

## Key Features

- **Multi-user support**: Multiple users can access their own isolated environments
- **Resource management**: Configure CPU, memory, and GPU resources per user
- **Customizable environments**: Choose from various pre-built images or use custom images
- **Persistent storage**: User data persists across sessions
- **Integration**: Works seamlessly with NRP's authentication and resource management

## Accessing JupyterHub

JupyterHub instances are typically accessed via a web interface. The URL format is usually:
- `https://<instance-name>.nrp-nautilus.io`

NRP has two JupyterHub instances:
- `https://jupyterhub-west.nrp-nautilus.io`
- `https://jupyterhub-east.nrp-nautilus.io`

You can find available JupyterHub instances through:
1. The NRP Portal
2. Your namespace administrator
3. Direct links provided by your instructor


## Hands-On: Launch a JupyterHub Session

1. Navigate to your JupyterHub URL (provided by your instructor or found in the NRP Portal)
2. Log in with your Authentik credentials
3. Select your server options (CPU, memory, image type)
4. Click "Start" to launch your server
5. Once started, you'll be redirected to your Jupyter environment

## Working with JupyterHub

### Available Image Options

JupyterHub typically offers several pre-configured images:
- **Scipy**: Scientific Python stack
- **R**: R statistical computing
- **Julia**: Julia programming language
- **Tensorflow**: Deep learning with TensorFlow
- **PyTorch**: Deep learning with PyTorch
- **Data Science**: Combined scipy, Julia, and R
- **PySpark**: Apache Spark with Python
- **All Spark**: Full Spark stack

### Resource Selection

When starting a server, you can typically select:
- **CPU cores**: Number of CPU cores (e.g., 1, 2, 4, 8)
- **Memory**: Amount of RAM (e.g., 2GB, 4GB, 8GB, 16GB)
- **GPU**: Optional GPU selection (if available in your namespace)
- **Storage**: Persistent home directory size


## Managing Your JupyterHub Server

You can manage your server from the JupyterHub interface:

### Stop Your Server
- Click "Control Panel" in the top right
- Click "Stop My Server" to stop your current session
- Your data will be preserved in your home directory

### Restart Your Server
- If your server is stopped, click "Start My Server" to restart it
- You'll be able to access all your previous work

### Change Server Options
- Stop your server first
- Click "Start My Server" and select different options
- Note: Some changes may require administrator approval


## Jupyter AI Integration

Jupyter AI brings generative AI capabilities directly into your Jupyter notebooks. With Jupyter AI, you can:

- **Chat with your data**: Ask questions about your notebooks, code, and data
- **Generate code**: Use AI to write code, fix bugs, and explain complex concepts
- **Transform notebooks**: Convert between formats, translate code, and more
- **Work with multiple models**: Connect to various LLM providers including NRP-managed LLMs

### Using Jupyter AI

Jupyter AI is typically pre-installed in JupyterHub environments. You can access it through:

1. **Jupyter AI Chat Interface**: A chat panel in your JupyterLab interface
2. **Magic commands**: Use `%%ai` magic commands directly in notebook cells
3. **Python API**: Programmatic access to AI features

**Note**: During the live tutorial, we will demonstrate Jupyter AI interactively. You'll see how to:
- Set up Jupyter AI with NRP-managed LLMs
- Use chat interface for code assistance
- Generate and explain code using AI
- Work with your data using natural language queries

For more information, see the [Jupyter AI documentation](https://jupyter-ai.readthedocs.io/).


# Part 3: Using Coder

## Overview

Coder provides a full VS Code experience in the browser, allowing you to develop, debug, and collaborate on code directly from your web browser. It's ideal for software development, data science, and collaborative coding.

## Key Features

- **Full VS Code**: Complete VS Code interface in the browser
- **Extensions**: Install and use VS Code extensions
- **Terminal**: Integrated terminal access
- **Git Integration**: Full Git support for version control
- **Multi-language**: Support for Python, JavaScript, Go, Rust, and more
- **Collaboration**: Share workspaces with team members

## Accessing Coder

NRP provides a hosted Coder instance:
- **NRP Coder**: [https://coder.nrp-nautilus.io/](https://coder.nrp-nautilus.io/)

You can also find Coder instances through:

You can find available Coder instances through:
1. The NRP Portal
2. Your namespace administrator
3. Direct links provided by your instructor


## Hands-On: Launch a Coder Workspace

1. Navigate to your Coder URL
2. Log in with your Authentik credentials
3. Create a new workspace or select an existing one
4. Choose your workspace template (Python, Node.js, Go, etc.)
5. Configure resources (CPU, memory, GPU if needed)
6. Click "Create" to launch your workspace

## Working with Coder

### VS Code Features

Once your workspace is running, you have access to:
- **File Explorer**: Navigate and manage files
- **Integrated Terminal**: Access to bash/zsh terminal
- **Source Control**: Git integration for version control
- **Extensions**: Install VS Code extensions from the marketplace
- **Debugger**: Full debugging capabilities
- **IntelliSense**: Code completion and suggestions

### Resource Management

Similar to JupyterHub, you can:
- **Stop Workspace**: Stop your workspace to free resources
- **Restart Workspace**: Restart to apply changes
- **Resize Workspace**: Adjust CPU/memory (may require admin approval)


# Part 4: Deploying JupyterHub

## Overview

This section covers how to deploy and manage JupyterHub environments for groups or courses. **Note: This section requires namespace admin privileges.**

## Prerequisites

- ✅ Namespace admin access
- ✅ Helm 3.x installed
- ✅ kubectl configured with admin access to your namespace
- ✅ Understanding of Kubernetes basics (from Beginner Track)

## Key Concepts

- **Helm Charts**: Package managers for Kubernetes applications
- **JupyterHub Helm Chart**: Pre-configured deployment for JupyterHub
- **Configuration**: Customize JupyterHub through YAML configuration files
- **Resource Management**: Set CPU, memory, and GPU limits per user
- **Storage**: Configure persistent storage for user home directories


## Section 4.1: Install Helm

First, ensure Helm is installed on your system:



```python
# Check if Helm is installed
helm version

```

If Helm is not installed, install it:

**macOS:**
```bash
brew install helm
```

**Linux:**
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

**Windows:**
Download from https://helm.sh/docs/intro/install/


## Section 4.2: Add JupyterHub Helm Repository

Add the JupyterHub Helm repository:



```python
# Add JupyterHub Helm repository
helm repo add jupyterhub https://jupyterhub.github.io/helm-chart/

```


```python
# Update Helm repositories
helm repo update

```


```python
# Verify the repository was added
helm repo list

```

## Section 4.3: Basic JupyterHub Deployment

Link to full documentation: https://nrp.ai/documentation/userdocs/jupyter/jupyterhub/

Let's create a basic JupyterHub configuration. First, create a configuration file:


### Create Basic JupyterHub Configuration

Create a file called `jupyterhub-config.yaml`:



```python
hub:
  config:
    JupyterHub:
      authenticator_class: dummy
      admin_access: true
      admin_users: ["admin"]
    DummyAuthenticator:
      password: "training123"
    # Allow all users to sign in (for tutorial purposes)
    Authenticator:
      allowed_users: set()
  service:
    type: ClusterIP
    annotations: {}
  deploymentStrategy:
    type: Recreate
  db:
    type: sqlite-pvc
    pvc:
      accessModes:
        - ReadWriteOnce
      storage: 1Gi
      storageClassName: rook-ceph-block
  resources:
    limits:
      cpu: "2"
      memory: 1Gi
    requests:
      cpu: 100m
      memory: 512Mi
  networkPolicy:
    enabled: false

proxy:
  secretToken: "REPLACE_WITH_GENERATED_TOKEN"
  service:
    type: ClusterIP
  chp:
    resources:
      limits:
        cpu: "6"
        memory: 10Gi
      requests:
        cpu: "1"
        memory: 512Mi

singleuser:
  uid: 0
  fsGid: 100
  extraEnv:
    GRANT_SUDO: "yes"
  image:
    name: quay.io/jupyter/scipy-notebook
    tag: 2024-04-22
  startTimeout: 600
  cpu:
    limit: 3
    guarantee: 3
  memory:
    limit: 10G
    guarantee: 10G
  storage:
    type: dynamic
    capacity: 5Gi
    dynamic:
      storageClass: rook-ceph-block
      pvcNameTemplate: claim-{username}{servername}
      volumeNameTemplate: volume-{username}{servername}
      storageAccessModes: [ReadWriteOnce]

# Required culling configuration (mandatory for NRP cluster policies)
cull:
  enabled: true
  users: false
  removeNamedServers: false
  timeout: 3600      # 1 hour in seconds - Must be ≤ 21600 (6 hours)
  every: 600         # Check every 10 minutes
  concurrency: 10    # Number of parallel culling operations
  maxAge: 0          # No maximum age limit

```

### Generate Secret Token

Before deploying, you need to generate a secret token for the proxy:



```python
# Generate a secret token
openssl rand -hex 32

```

**Important**: Replace `REPLACE_WITH_GENERATED_TOKEN` in the `jupyterhub-config.yaml` file with the token generated above.


### Deploy JupyterHub

Now deploy JupyterHub using Helm. **Important**: Replace `<namespace>` with your namespace name.



```python
# Deploy JupyterHub
# Replace <namespace> with your namespace
# Replace <release-name> with a name for your JupyterHub instance (e.g., jhub-basic)

helm upgrade --cleanup-on-fail --install <release-name> jupyterhub/jupyterhub \
    --namespace <namespace> \
    --version=3.3.7 \
    --values jupyterhub-config.yaml \
    --wait \
    --timeout=10m

```


```python
# Check deployment status
kubectl get pods -n <namespace>

```


```python
# Check services
kubectl get services -n <namespace>

```

## Section 4.4: Configure Ingress

To make JupyterHub accessible via a URL, create an Ingress resource:


### Create Ingress Configuration

Add the following to your `jupyterhub-config.yaml` or create a separate ingress file:



```python
ingress:
  enabled: true
  ingressClassName: haproxy
  hosts: ["<your-jupyterhub-name>.nrp-nautilus.io"]
  pathSuffix: ''
  tls:
    - hosts:
      - <your-jupyterhub-name>.nrp-nautilus.io

```

**Note**: Replace `<your-jupyterhub-name>` with your desired subdomain. You may need to coordinate with NRP administrators to set up the DNS.

After adding the ingress configuration, upgrade your Helm release:



```python
# Upgrade the deployment with ingress configuration
helm upgrade <release-name> jupyterhub/jupyterhub \
    --namespace <namespace> \
    --version=3.3.7 \
    --values jupyterhub-config.yaml \
    --wait \
    --timeout=10m

```


```python
# Check ingress status
kubectl get ingress -n <namespace>

```

## Section 4.5: Advanced Configuration

### Multiple Image Profiles

You can configure multiple image options for users to choose from. Add this to your `jupyterhub-config.yaml`:



```python
singleuser:
  profileList:
  - display_name: Scipy
    kubespawner_override:
      image_spec: quay.io/jupyter/scipy-notebook:2024-04-22
    default: True
  - display_name: R
    kubespawner_override:
      image_spec: quay.io/jupyter/r-notebook:2024-04-22
  - display_name: Julia
    kubespawner_override:
      image_spec: quay.io/jupyter/julia-notebook:2024-04-22
  - display_name: Tensorflow
    kubespawner_override:
      image_spec: quay.io/jupyter/tensorflow-notebook:cuda-2024-04-22
  - display_name: Pytorch
    kubespawner_override:
      image_spec: quay.io/jupyter/pytorch-notebook:cuda12-2024-04-22
  - display_name: Datascience (scipy, Julia, R)
    kubespawner_override:
      image_spec: quay.io/jupyter/datascience-notebook:2024-04-22

```

### Resource Limits per Profile

You can also set different resource limits for different profiles:



```python
singleuser:
  profileList:
  - display_name: Small (2 CPU, 4GB RAM)
    kubespawner_override:
      image_spec: quay.io/jupyter/scipy-notebook:2024-04-22
      cpu_limit: 2
      cpu_guarantee: 2
      mem_limit: 4G
      mem_guarantee: 4G
  - display_name: Medium (4 CPU, 8GB RAM)
    kubespawner_override:
      image_spec: quay.io/jupyter/scipy-notebook:2024-04-22
      cpu_limit: 4
      cpu_guarantee: 4
      mem_limit: 8G
      mem_guarantee: 8G
  - display_name: Large (8 CPU, 16GB RAM)
    kubespawner_override:
      image_spec: quay.io/jupyter/scipy-notebook:2024-04-22
      cpu_limit: 8
      cpu_guarantee: 8
      mem_limit: 16G
      mem_guarantee: 16G

```

## Section 4.6: Managing JupyterHub

### Check JupyterHub Status



```python
# List all Helm releases in your namespace
helm list -n <namespace>

```


```python
# Check JupyterHub hub pod logs
kubectl logs -n <namespace> -l app=jupyterhub,component=hub --tail=50

```


```python
# Check all user pods
kubectl get pods -n <namespace> -l app=jupyterhub,component=singleuser-server

```

### Update JupyterHub Configuration

To update your JupyterHub configuration:

1. Edit your `jupyterhub-config.yaml` file
2. Upgrade the Helm release:



```python
# Upgrade JupyterHub with new configuration
helm upgrade <release-name> jupyterhub/jupyterhub \
    --namespace <namespace> \
    --version=3.3.7 \
    --values jupyterhub-config.yaml \
    --wait \
    --timeout=10m

```

### Uninstall JupyterHub

To remove JupyterHub (and optionally all user data):



```python
# Uninstall JupyterHub (this will delete the hub and proxy, but NOT user data)
helm uninstall <release-name> -n <namespace>

```


```python
# To also delete user PVCs (be careful - this deletes all user data!)
# kubectl delete pvc -n <namespace> -l app=jupyterhub,component=singleuser-storage

```

## Section 4.7: Troubleshooting

### Common Issues

**JupyterHub not starting:**
```bash
# Check hub pod logs
kubectl logs -n <namespace> -l app=jupyterhub,component=hub

# Check proxy pod logs
kubectl logs -n <namespace> -l app=jupyterhub,component=proxy
```

**User pods not starting:**
```bash
# Check events
kubectl get events -n <namespace> --sort-by=.metadata.creationTimestamp

# Describe the failing pod
kubectl describe pod <pod-name> -n <namespace>
```

**Storage issues:**
```bash
# Check PVC status
kubectl get pvc -n <namespace>

# Check storage class
kubectl get storageclass
```


# Part 5: Building in GitLab

## Overview

NRP provides GitLab integration for building container images and automating CI/CD pipelines. This section covers how to use GitLab for building and deploying container images on NRP.

For comprehensive documentation, see: [https://nrp.ai/documentation/userdocs/development/gitlab/](https://nrp.ai/documentation/userdocs/development/gitlab/)

## Key Features

- **Container Image Building**: Build Docker images directly in GitLab CI/CD
- **Kubernetes Integration**: Deploy applications to your namespace from GitLab pipelines
- **Automated Workflows**: Set up CI/CD pipelines for your projects
- **Private Repositories**: Support for private GitLab repositories
- **Custom Images**: Build and use custom images in JupyterHub and other services

## Prerequisites

- ✅ Access to NRP GitLab instance
- ✅ Namespace admin privileges (for deploying to Kubernetes)
- ✅ Understanding of Git and GitLab CI/CD basics
- ✅ Docker/container image concepts

## Getting Started with GitLab

### Access GitLab

NRP provides a GitLab instance for building images and managing repositories. Access it through:
- The NRP Portal
- Direct link provided by your namespace administrator

### Create a Project

1. Create a new GitLab project or use an existing one
2. Add your code and Dockerfile
3. Configure CI/CD pipeline using `.gitlab-ci.yml`

## Building Container Images

### Basic GitLab CI/CD Pipeline

Create a `.gitlab-ci.yml` file in your repository:

```yaml
image: ghcr.io/osscontainertools/kaniko:debug

stages:
- build-and-push

build-and-push-job:
  stage: build-and-push
  variables:
    GODEBUG: "http2client=0"
  script:
  - echo "{\"auths\":{\"$CI_REGISTRY\":{\"username\":\"$CI_REGISTRY_USER\",\"password\":\"$CI_REGISTRY_PASSWORD\"}}}" > /kaniko/.docker/config.json
  - /kaniko/executor --cache=true --push-retry=10 --context $CI_PROJECT_DIR --dockerfile $CI_PROJECT_DIR/Dockerfile --destination $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA --destination $CI_REGISTRY_IMAGE:latest
```

### Using Built Images

Once your image is built, you can use it in:
- **JupyterHub**: Configure custom images in your JupyterHub deployment
- **Kubernetes Pods**: Reference the image in your pod specifications
- **Jobs**: Use in batch jobs and other workloads

## Kubernetes Integration

GitLab can deploy directly to your Kubernetes namespace:

1. **Create Service Account**: Set up a service account with appropriate permissions
2. **Configure GitLab**: Add Kubernetes cluster information to GitLab
3. **Deploy Pipeline**: Add deployment stages to your CI/CD pipeline

For detailed steps, see the [GitLab Kubernetes Integration documentation](https://nrp.ai/documentation/userdocs/development/gitlab/).

## Best Practices

- **Use Tags**: Tag your images with version numbers or commit SHAs
- **Cache Layers**: Optimize Docker builds with layer caching
- **Security**: Keep your GitLab tokens and credentials secure
- **Testing**: Test images before deploying to production
- **Documentation**: Document your build process and image usage

## Resources

- [NRP GitLab Documentation](https://nrp.ai/documentation/userdocs/development/gitlab/)
- [GitLab CI/CD Documentation](https://docs.gitlab.com/ee/ci/)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)


---

# End of Intermediate Track

Congratulations! You've completed the Intermediate Track training. You should now be familiar with:

- ✅ Using JupyterHub as a user
- ✅ Using Jupyter AI for AI-assisted development
- ✅ Using Coder for full IDE experience
- ✅ Using NRP-managed LLMs via API and chat interfaces
- ✅ Deploying and managing JupyterHub instances
- ✅ Configuring JupyterHub with Helm
- ✅ Building container images in GitLab
- ✅ Managing resources and user environments
- ✅ Troubleshooting common issues

**Next Steps**: 
- Practice deploying JupyterHub in your own namespace
- Explore advanced JupyterHub configurations
- Build custom images in GitLab for your projects
- Integrate NRP LLMs into your applications
- Review the [NRP Documentation](https://docs.nrp.ai/) for more advanced features

