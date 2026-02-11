# NRP Training - Beginner Track

**February 2026 Training**

This notebook covers the Beginner Track topics for the National Research Platform (NRP) training:
1. Introduction to the National Research Platform
2. The Portal: GUI Access to Resources
3. Basic and Intermediate Docker and Kubernetes

---

## Facilitators

- **Daniel Diaz** - Reachable on Matrix
- **Mohammad Firas Sada** - Reachable on Matrix
- **Dmitry Mishin** - Reachable on Matrix

### Getting Help

- **Matrix**: Join NRP's Matrix for user support and real-time updates. Steps to join are available at [https://nrp.ai/contact/](https://nrp.ai/contact/)
- **Email**: Alternatively, contact us at usersupport@nrp-nautilus.io

**Important**: Don't try contacting admins in Matrix by private messages, those will be rejected. Use public rooms for general support questions.

---

## Tutorial Environment

This tutorial is expected to run on a **local environment** with `kubectl` installed. Full setup steps are available at [https://nrp.ai/documentation/userdocs/start/getting-started/](https://nrp.ai/documentation/userdocs/start/getting-started/).

### Live Tutorial Option

The live tutorial will be conducted using **NRP's JupyterHub** at `jupyterhub-west.nrp-nautilus.io` as the local environment. 

**To use NRP's JupyterHub, you need to:**
- Be part of at least one namespace
- Use that namespace for the tutorial
- Check your namespaces at [https://nrp.ai/namespaces/](https://nrp.ai/namespaces/)

### Namespace Requirements

**If you're not part of a namespace, you need to get access:**

1. **If you are a student**: Contact your research supervisor and ask them to add you to their namespace. Once added, your status will change to a cluster **`user`** and you will get access to all namespace resources.

2. **If you are a faculty member, researcher, or postdoc**: Starting a new project and need your own namespace (either for yourself or your research group), you can request to be promoted to a namespace **`admin`** in [Matrix](https://nrp.ai/contact/).

   As an **`admin`**, you will have the ability to:
   - Create multiple namespaces
   - Invite other users to your namespace(s)

   **Admin Responsibilities**: As a namespace **`admin`**, you are responsible for:
   - All activity happening in your namespaces
   - Keeping the user list up-to-date

---

## Prerequisites

Before starting, ensure you have:
- NRP access in Authentik (required for hands-on exercises)
- Part of at least one namespace (check at [https://nrp.ai/namespaces/](https://nrp.ai/namespaces/))
- kubectl installed on your system (see installation steps below)
- kubelogin plugin installed (see installation steps below)
- Kubernetes config file set up at `~/.kube/config` (see configuration steps below)
- Access to the NRP Portal at https://nrp.ai/

---

## Resources

- [NRP Portal](https://nrp.ai/)
- [NRP Documentation](https://docs.nrp.ai/)
- [NRP Training Schedule](https://nrp.ai/training/)
- [NRP Contact/Matrix](https://nrp.ai/contact/)
- [Getting Started Guide](https://nrp.ai/documentation/userdocs/start/getting-started/)


# Part 0: Installing and Configuring kubectl

We've prepared Pre-recorded Videos on YouTube to show how to install Kubectl and Kubelogin on each platform here: https://www.youtube.com/watch?v=zZBDPc0RieU&list=PL3p1Zps9-ohB4jE8VRfyS0xjzqgUAFx-Y

## Overview

Before we begin, you need to install and configure `kubectl` (the Kubernetes command-line tool) and the `kubelogin` plugin on your local machine. These steps are essential for accessing the NRP cluster.

---

## Step 1: Install kubectl

Install the Kubernetes command-line tool, `kubectl`.

**For Linux:**
```bash
# Download the latest stable release
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# Install kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

**For macOS:**
```bash
# Using Homebrew
brew install kubectl
```

**For Windows:**
Download from [https://kubernetes.io/docs/tasks/tools/install-kubectl/](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

**Verify installation:**
```bash
kubectl version --client
```

---

## Step 2: Install kubelogin Plugin

**⚠️ Required**: You **must** install the `kubelogin` plugin, or your kubeconfig file **will not work**.

**For Linux/macOS:**
```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install kubelogin
brew install kubelogin
```

**Alternative for Linux (using wget):**
```bash
# Download kubelogin
wget https://github.com/int128/kubelogin/releases/latest/download/kubelogin_linux_amd64.zip
unzip kubelogin_linux_amd64.zip
sudo mv kubelogin /usr/local/bin/
```

**For Windows:**
Download from [https://github.com/int128/kubelogin](https://github.com/int128/kubelogin)

**Verify installation:**
```bash
kubectl oidc-login --version
```

---

## Step 3: Download and Configure Kubernetes Config File

1. **Download the config file** from [https://nrp.ai/documentation/userdocs/start/getting-started/](https://nrp.ai/documentation/userdocs/start/getting-started/) (look for "Download Config File" button)

2. **Create the `.kube` directory** (if it doesn't exist):


## Hands-On: Verify Your Access

Let's verify that you have proper access to the cluster. Copy and paste these commands into your terminal:



```python
# Check kubectl version
kubectl version --client

```


      Cell In[1], line 2
        kubectl version --client
                ^
    SyntaxError: invalid syntax




```python
# Check current context and namespace
kubectl config current-context

```


```python
# Test cluster access - list nodes
kubectl get nodes

```

# Part 2: The Portal - GUI Access to Resources

## Overview

The NRP Portal provides a web-based interface for managing your NRP resources, including namespaces, groups, and integrated AI/LLM services.

## Key Features

### Portal Navigation
- **Dashboard**: Overview of your resources and usage
- **Namespace Management**: Create, view, and manage namespaces
- **Group Management**: Organize users into groups for shared resources
- **Resource Monitoring**: View cluster resources, GPU availability, and usage statistics

### LLM Interface
- Integrated access to Large Language Models
- Token management for LLM services
- Direct API access from the portal

## Hands-On: Explore the Portal

1. Navigate to https://nrp.ai/
2. Log in with your Authentik credentials
3. Explore the following sections:
   - **User Info**: View your account details and namespace access
   - **Namespaces**: See your available namespaces and their resources
   - **Reservations**: View any resource reservations you have


## Check Your Namespace Access

You can also check your namespace access from the command line:



```python
# If you know you’re a member of a namespace, you can set it as default.
kubectl config set contexts.nautilus.namespace <YOUR NAMESPACE>

```

# Part 3: Basic and Intermediate Docker and Kubernetes

## Overview

This section covers the fundamentals of container orchestration with Kubernetes on the NRP platform, including Pods, Deployments, Persistent Volume Claims (PVCs), S3 storage, and advanced scheduling features.

---

## Section 3.1: Exploring the Cluster

Let's start by exploring what resources are available in the cluster.



```python
# List all nodes in the cluster
kubectl get nodes

```


```python
# Check GPU availability - list nodes with GPU product labels
kubectl get nodes -L nvidia.com/gpu.product

```


```python
# Count available GPU types
kubectl get nodes -L nvidia.com/gpu.product --no-headers | awk '{print $NF}' | sort | uniq -c

```


```python
# List available regions and zones
kubectl get nodes -L topology.kubernetes.io/region,topology.kubernetes.io/zone

```

## Section 3.2: Working with Pods

Pods are the smallest deployable units in Kubernetes. Let's create and manage a simple pod.

**Important**: Replace `<username>` with your actual username in the YAML files to avoid name collisions in shared namespaces.


### Create a Simple Pod

First, create a file called `test-pod.yaml` with the following content:



```python
apiVersion: v1
kind: Pod
metadata:
  name: test-pod-<username>
spec:
  containers:
  - name: mypod
    image: ubuntu
    resources:
      limits:
        memory: 100Mi
        cpu: 100m
      requests:
        memory: 100Mi
        cpu: 100m
    command: ["sh", "-c", "echo 'Hello from NRP!' && sleep 3600"]

```

**Remember to replace `<username>` with your actual username!** You can create this file using:

```bash
cat > test-pod.yaml << 'EOF'
# Paste the YAML above here, replacing <username>
EOF
```



```python
# Create the pod
kubectl create -f test-pod.yaml

```


```python
# Check pod status
kubectl get pods

```


```python
# Get detailed pod information
kubectl get pod test-pod-<username> -o wide

```


```python
# View pod logs
kubectl logs test-pod-<username>

```


```python
# Execute a command in the pod
kubectl exec test-pod-<username> -- echo 'Command executed successfully'

```


```python
# Get interactive shell access to the pod
kubectl exec -it test-pod-<username> -- /bin/bash

```

### Cleanup

When you're done exploring, delete the pod:



```python
# Delete the pod
kubectl delete pod test-pod-<username>

```

## Section 3.3: Working with Jobs

Jobs are used for batch workloads that need to run to completion. Let's create a simple job.


### Create a Simple Job

Create a file called `simple-job.yaml` with the following content:



```python
apiVersion: batch/v1
kind: Job
metadata:
  name: simple-job-<username>
spec:
  template:
    spec:
      containers:
      - name: simple-container
        image: ubuntu
        command: ["sh", "-c", "echo 'Hello from NRP Job!' && date && exit 0"]
        resources:
          limits:
            memory: 100Mi
            cpu: 100m
          requests:
            memory: 100Mi
            cpu: 100m
      restartPolicy: Never
  backoffLimit: 1
  ttlSecondsAfterFinished: 300

```


      Cell In[2], line 14
        memory: 100Mi
                  ^
    SyntaxError: invalid decimal literal




```python
# Create the job
kubectl create -f simple-job.yaml

```


```python
# Check job status
kubectl get jobs

```


```python
# Check the pods created by the job
kubectl get pods -l job-name=simple-job-<username>

```


```python
# View job logs (replace <pod-name> with the actual pod name from above)
kubectl logs <pod-name>

```

## Section 3.4: Working with Deployments

Deployments manage a set of identical pods and provide rolling updates and rollback capabilities.


### Create a Deployment

Create a file called `http-deployment.yaml`:



```python
apiVersion: apps/v1
kind: Deployment
metadata:
  name: test-http-<username>
  labels:
    k8s-app: test-http
spec:
  replicas: 2
  selector:
    matchLabels:
      k8s-app: test-http
  template:
    metadata:
      labels:
        k8s-app: test-http
    spec:
      containers:
      - name: mypod
        image: httpd:alpine
        resources:
           limits:
             memory: 200Mi
             cpu: 1
           requests:
             memory: 50Mi
             cpu: 50m
        ports:
        - containerPort: 80

```


```python
# Create the deployment
kubectl create -f http-deployment.yaml

```


```python
# Check deployment status
kubectl get deployments

```


```python
# Check the pods created by the deployment
kubectl get pods -l k8s-app=test-http

```

## Section 3.5: Exposing Services

Services provide stable network access to pods. Let's create a service and expose it via Ingress.


### Create a Service

Create a file called `service.yaml`:



```python
apiVersion: v1
kind: Service
metadata:
  labels:
    k8s-app: test-svc
  name: test-svc-<username>
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    k8s-app: test-http
  type: ClusterIP

```


```python
# Create the service
kubectl create -f service.yaml

```


```python
# View the service
kubectl get services

```

### Create an Ingress

Create a file called `ingress.yaml`:



```python
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: test-ingress-<username>
spec:
  ingressClassName: haproxy
  rules:
  - host: test-service-<username>.nrp-nautilus.io
    http:
      paths:
      - backend:
          service:
            name: test-svc-<username>
            port:
              number: 80
        path: /
        pathType: ImplementationSpecific
  tls:
  - hosts:
    - test-service-<username>.nrp-nautilus.io

```


```python
# Create the ingress
kubectl create -f ingress.yaml

```


```python
# Check ingress status
kubectl get ingress

```

## Section 3.6: Persistent Volume Claims (PVCs)

More info on NRP's Storage Classes: https://nrp.ai/documentation/userdocs/storage/ceph/

PVCs provide persistent storage for your workloads. NRP supports multiple storage classes including `rook-ceph-block` (ReadWriteOnce) and `rook-cephfs` (ReadWriteMany).


### Create a CephFS PVC (ReadWriteMany)

Create a file called `cephfs-pvc.yaml`:



```python
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: cephfs-pvc-<username>
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
  storageClassName: rook-cephfs

```


```python
# Create the PVC
kubectl create -f cephfs-pvc.yaml

```


```python
# Check PVC status (should show Bound)
kubectl get pvc

```

### Create a Pod Using the PVC

Create a file called `cephfs-pod.yaml`:



```python
apiVersion: v1
kind: Pod
metadata:
  name: cephfs-pod-<username>
spec:
  containers:
  - name: cephfs-container
    image: ubuntu
    command: ["sh", "-c", "echo 'Data written at $(date)' > /shared-data/test.txt && cat /shared-data/test.txt && sleep 3600"]
    volumeMounts:
    - name: cephfs-volume
      mountPath: /shared-data
  volumes:
  - name: cephfs-volume
    persistentVolumeClaim:
      claimName: cephfs-pvc-<username>

```


```python
# Create the pod
kubectl create -f cephfs-pod.yaml

```


```python
# Check pod logs to see the data written
kubectl logs cephfs-pod-<username>

```

## Section 3.7: Node Affinity and Scheduling

Node affinity allows you to control which nodes your pods are scheduled on. This is useful for GPU selection, region/zone preferences, and other resource requirements.


### Create a Pod with Node Affinity

Create a file called `region-pod.yaml`:



```python
apiVersion: v1
kind: Pod
metadata:
  name: region-test-<username>
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: topology.kubernetes.io/region
            operator: In
            values:
            - us-west
  containers:
  - name: mypod
    image: alpine/git:v2.49.1
    resources:
      limits:
        memory: 1Gi
        cpu: 1
      requests:
        memory: 100Mi
        cpu: 100m
    command: ["sh", "-c", "echo 'Pod running in us-west region' && sleep 600"]

```


```python
# Create the pod
kubectl create -f region-pod.yaml

```


```python
# Verify the pod was scheduled in the correct region
kubectl get pod region-test-<username> -o wide

```

### GPU Node Affinity Example

Create a file called `gpu-pod.yaml` to request a specific GPU type:



```python
apiVersion: v1
kind: Pod
metadata:
  name: gpu-pod-<username>
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: nvidia.com/gpu.product
            operator: In
            values:
            - NVIDIA-A10
  containers:
  - name: gpu-container
    image: nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04
    command: ["sleep", "3600"]
    resources:
      limits:
        nvidia.com/gpu: 1
        memory: 4Gi
        cpu: 2
      requests:
        nvidia.com/gpu: 1
        memory: 4Gi
        cpu: 2
    volumeMounts:
    - mountPath: /dev/shm
      name: dshm
  volumes:
  - name: dshm
    emptyDir:
      medium: Memory
      sizeLimit: 2Gi

```


```python
# Create the GPU pod
kubectl create -f gpu-pod.yaml

```


```python
# Check pod status
kubectl get pod gpu-pod-<username> -o wide

```


```python
# Once the pod is running, you can exec into it and check GPU
kubectl exec gpu-pod-<username> -- nvidia-smi

```

## Section 3.8: Priority Classes and Taints/Tolerations

Priority classes allow you to run low-priority workloads that can be preempted by higher priority jobs.


### Create a Low Priority Job

Create a file called `low-priority-job.yaml`:



```python
apiVersion: batch/v1
kind: Job
metadata:
  name: low-priority-job-<username>
  labels:
    app: low-priority-job
spec:
  template:
    spec:
      priorityClassName: opportunistic
      containers:
      - name: test-container
        image: nginx:latest
        command: ["/bin/sh"]
        args: ["-c", "echo 'Low priority job completed' && sleep 30"]
        resources:
          requests:
            cpu: "100m"
            memory: "128Mi"
          limits:
            cpu: "200m"
            memory: "171Mi"
      restartPolicy: Never
  backoffLimit: 3

```


```python
# Create the low priority job
kubectl create -f low-priority-job.yaml

```


```python
kubectl create serviceaccount <service-account-name>
```


```python
kubectl apply -f - <<EOF
apiVersion: v1
kind: Secret
metadata:
  name: <service-account-name>-secret
  annotations:
    kubernetes.io/service-account.name: <service-account-name>
type: kubernetes.io/service-account-token
EOF
```


```python
TOKEN=`kubectl get secret <service-account-name>-secret -o jsonpath='{.data.token}'| base64 --decode`
echo $TOKEN
```


```python
kubectl create rolebinding <service-account-name>-sa --clusterrole=<admin OR edit> --serviceaccount=<namespace>:<service-account-name>
```

## Section 3.9: S3 Storage

S3 storage is useful for large datasets that can be accessed from multiple locations. NRP provides S3-compatible storage.


Docs are here: https://nrp.ai/documentation/userdocs/storage/ceph-s3/

### Access S3 from a Pod

Create a file called `s3-pod.yaml`:



```python
apiVersion: v1
kind: Pod
metadata:
  name: s3-pod-<username>
spec:
  containers:
  - name: awscli
    image: amazon/aws-cli:2.31.36
    resources:
      limits:
        memory: 10Gi
        cpu: 4
        ephemeral-storage: 100Gi
      requests:
        memory: 10Gi
        cpu: 4
        ephemeral-storage: 100Gi
    command: ["/bin/bash", "-c"]
    args:
    - >-
        yum -y install python3-pip wget gzip tar;
        pip3 install boto3 torch torchvision;
        echo "Setup complete. Ready for S3 operations.";
        sleep 3600s;
    volumeMounts:
    - name: scratch
      mountPath: /scratch
  volumes:
  - name: scratch
    emptyDir: {}

```


```python
# Create the S3 pod
kubectl create -f s3-pod.yaml

```


```python
# Wait for pod to be ready, then exec into it
kubectl wait --for=condition=ready pod/s3-pod-<username> --timeout=300s

```


```python
# Get interactive access to the pod
kubectl exec -it s3-pod-<username> -- /bin/bash

```

## Cleanup

**Important**: Always clean up your resources when you're done! Make sure you don't leave any running pods or jobs.

Here are some useful cleanup commands:



```python
# List all your pods
kubectl get pods

```


```python
# Delete all pods (be careful!)
# kubectl delete pods --all

```

## Section 3.10: RAG example using Ollama

An Ollama example:

```bash
apiVersion: v1
kind: Pod
metadata:
  name: ollama-<username>
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: nvidia.com/gpu.product
            operator: In
            values:
            - NVIDIA-A10
  volumes:
  - name: scratch
    emptyDir: {}
  - name: shm
    emptyDir:
      medium: Memory
      sizeLimit: 8Gi
  containers:
  - name: mypod
    image: ollama/ollama:0.2.8
    resources:
      limits:
        memory: 24Gi
        cpu: 4
        nvidia.com/gpu: 1
      requests:
        memory: 24Gi
        cpu: 4
        nvidia.com/gpu: 1
    command: ["/bin/bash", "-c"]
    args:
    - >-
        apt update;
        apt install -y pip wget vim;
        pip3 install -U langchain-cli==0.0.31;
        pip3 install -U langchain-community==0.3.7;
        pip3 install -U chromadb==0.5.18;
        pip3 install -U sentence-transformers==2.2.2 transformers==4.46.2;
        pip3 install -U huggingface_hub==0.25.2;
        export HOME=/scratch; 
        cd $HOME;
        wget https://www.gutenberg.org/cache/epub/55084/pg55084.txt;
        sleep 3600;
    volumeMounts:
            - name: scratch
              mountPath: /scratch
            - name: shm
              mountPath: /dev/shm
```

Start up the pod:
```
kubectl apply -f ollama-rag.yaml
```
Watch the logs and make sure you wait till the installs are done and the book is downloaded:

```
kubectl logs ollama-username
```
Once the book is downloaded (you will see wget output in the logs), we can get interactive access to the pod and start up the Ollama server and pull the module we want to use (Mistral):

```
kubectl exec -it ollama-username -- /bin/bash
cd /scratch
nohup ollama serve&
ollama pull mistral
```
We can now download our test script and run it:
```
wget https://raw.githubusercontent.com/mahidhar/training_k8s_tutorial/refs/heads/main/test.py
python3 -i test.py
```
Now we can run the rag within the interactive python interpreter. Do the following one by one (i.e. wait for results before moving to the next one)
```
rag.invoke("What do you feed pigeons ?")
rag.invoke("Do tame pigeons have better plumage ?")
rag.invoke("What affects pigeon plumage ?")
```

## Section 3.11: RAG example using Milvus

This example demonstrates RAG using Milvus as the vector database instead of ChromaDB. Milvus is a distributed vector database designed for scalable similarity search. In the milvus-rag.yaml file please change the username to a unique name for yourself.

A Milvus example:

```bash
apiVersion: v1
kind: Pod
metadata:
  name: vectordb-example-<username>
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: nvidia.com/gpu.product
            operator: In
            values:
            - NVIDIA-A10
  volumes:
  - name: scratch
    emptyDir: {}
  - name: shm
    emptyDir:
      medium: Memory
      sizeLimit: 8Gi
  containers:
  - name: mypod
    image: nvidia/cuda:12.1.0-runtime-ubuntu22.04
    resources:
      limits:
        memory: 24Gi
        cpu: 4
        nvidia.com/gpu: 1
      requests:
        memory: 24Gi
        cpu: 4
        nvidia.com/gpu: 1
    command: ["/bin/bash", "-c"]
    args:
    - >-
        apt update && apt install -y python3 python3-pip wget vim curl;
        pip3 install --upgrade pip;
        pip3 install -U pymilvus>=2.3.0 sentence-transformers>=2.2.0 torch>=2.0.0 numpy>=1.24.0 langchain>=0.1.0 langchain-community>=0.0.20 langchain-milvus>=0.1.0 langchain-classic>=1.0.0 langchain-text-splitters>=1.0.0;
        curl -fsSL https://ollama.com/install.sh | sh;
        export HOME=/scratch;
        cd $HOME;
        nohup ollama serve > /tmp/ollama.log 2>&1 &
        sleep 5;
        ollama pull mistral > /tmp/ollama-pull.log 2>&1 &
        echo "Setup complete! Ollama is starting and mistral model is downloading in background.";
        sleep 3600;
    env:
    - name: MILVUS_HOST
      valueFrom:
        secretKeyRef:
          name: training-milvus-credentials
          key: host
    - name: MILVUS_PORT
      valueFrom:
        secretKeyRef:
          name: training-milvus-credentials
          key: port
    - name: MILVUS_USER
      valueFrom:
        secretKeyRef:
          name: training-milvus-credentials
          key: username
    - name: MILVUS_PASSWORD
      valueFrom:
        secretKeyRef:
          name: training-milvus-credentials
          key: password
    - name: MILVUS_SECURE
      valueFrom:
        secretKeyRef:
          name: training-milvus-credentials
          key: secure
    - name: MILVUS_DB_NAME
      valueFrom:
        secretKeyRef:
          name: training-milvus-credentials
          key: database
    volumeMounts:
            - name: scratch
              mountPath: /scratch
            - name: shm
              mountPath: /dev/shm
```

Start up the pod:
```
kubectl apply -f milvus-rag.yaml
```
Watch the logs and make sure you wait till the installs are done:

```
kubectl logs vectordb-example-username
```
The pod automatically:
- Installs all Python dependencies
- Installs Ollama
- Starts Ollama server in the background
- Downloads the mistral model in the background

Download the simple example script into the pod once it is running:
```
kubectl exec -it vectordb-example-username -n -- /bin/bash
cd /scratch
wget https://raw.githubusercontent.com/groundsada/nrp-milvus-example/refs/heads/main/milvus-example.py
```

Once the installation is complete (check logs), you can run the example. \
**Note:** This example connects to the `training_milvus` database using credentials from the Kubernetes secret `training-milvus-credentials`. The collection name is `simple_rag_example`. You can change the name in the `milvus-example.py` file if you want to create a separate collection of your own. There are two places where the collection name is specified. Look for:

```
 collection_name="simple_rag_example"
```
For all other aspects, the script uses environment variables for Milvus connection, so no manual editing is needed.

```
kubectl exec -it vectordb-example-username -n training -- /bin/bash
cd /scratch
python3 milvus-example.py
```

This simple example:
- Uses a small set of sample documents to demonstrate Milvus vector storage and retrieval
- Shows RAG with Ollama LLM




---

# End of Beginner Track

Congratulations! You've completed the Beginner Track training. You should now be familiar with:

- ✅ Basic Kubernetes concepts (Pods, Jobs, Deployments)
- ✅ Services and Ingress for exposing applications
- ✅ Persistent storage with PVCs
- ✅ Node affinity and scheduling
- ✅ Priority classes
- ✅ Python Kubernetes API
- ✅ S3 storage access
- ✅ Provisioning LLMs with GPUs
- ✅ RAG and Vector Databases

**Next Steps**: Consider attending the Intermediate Track to learn about deploying and managing JupyterHub and Coder environments.



```python

```
