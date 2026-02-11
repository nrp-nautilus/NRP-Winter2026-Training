---
title: "Intro to Kubernetes and Docker"
teaching: 30
exercises: 30
questions:
- "[Intro to Kubernetes](#kubernetes-basics)"
- "[Intro to Docker and registries](#docker-and-containers)"
- "[How to work with a pod](#creating-a-simple-pod)"
- "[How to work with a job](#creating-a-simple-job)"
- "[How to work with a deployment](#creating-a-simple-deployment)"
hidden: True

---
## Requirements
This section will contain some hands-on activities that will require that you have completed the [Setup]({{ page.root }}/setup.html) page.

## Kubernetes basics
Kubernetes is a system for running applications on a cluster by managing **workloads** (things you want to run) and keeping them in the desired state.

Most interactions with Kubernetes involve creating and updating **resources** (objects) described in **YAML**.
- A YAML “manifest” declares the *desired state* (what you want running)
- Kubernetes works continuously to make the cluster match that desired state

Typical workflow:
1. Write or edit a YAML manifest
2. Apply it to the cluster (e.g., `kubectl apply -f ...`)
3. Check status and troubleshoot (pods, logs, events)

### Kubernetes workloads
Workloads are the resource types you use to run containers on the cluster.

- **Pod**: the basic unit where your application runs (one or more containers together)
- **Job**: runs work to completion (batch or one-off tasks)
- **Deployment**: manages long-running services and keeps them available (including rolling updates)

Rule of thumb:
- Use a **Job** when the work should finish.
- Use a **Deployment** when the work should keep running.


## Docker and containers
Docker is a tool for building and running **containers**.

A container image packages:
- your application code
- libraries and dependencies
- enough operating-system files to run consistently

This makes the environment portable: the same image can run on your laptop, a VM, or on a Kubernetes cluster.
### Why Docker matters for Kubernetes
Kubernetes runs **container images**. It does not build them.

In practice:
- You build a container image (with Docker or another tool)
- Kubernetes pulls that image and runs it as part of your workload

### Container registries
A **container registry** stores and distributes container images.

- Public example: Docker Hub
- Organizations often use private registries for internal images

NRP note:
- NRP GitLab provides a container registry (public or private depending on repo settings)
- You can push local images to GitLab’s registry, or build/publish images using GitLab CI/CD

## Hands on
In this session we will go through some commone kubectl commands and show you how to create some kubernetes objects.

### Creating a simple pod
Edit the following `test-pod.yaml` file to give the pod a unique name.
```yaml
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
- Now, launch this pod with the following
```bash
kubectl create -f test-pod.yaml
```
- Check if you were successful by running 
```bash
kubectl get pods
# Get detailed pod information
kubectl get pod test-pod-<username> -o wide
```
- Have a look at the logs associated with this pod
```bash
# View pod logs
kubectl logs test-pod-<username>
```
- View detailed pod information by running
```bash
# Print pod details
kubectl describe pod test-pod-<username>
```
- Execute a command from the pod
```bash
# Execute a command in the pod
kubectl exec test-pod-<username> -- echo 'Command executed successfully'
```
- Shell into the pod for an interactive session
```bash
# Get interactive shell access to the pod
kubectl exec -it test-pod-<username> -- /bin/bash
```
- Finally clean up the pod to free up any resources
```bash
# Delete the pod
kubectl delete pod test-pod-<username>
```

### Creating a simple job
Jobs are used for batch workloads that need to run to completion. Let's create a simple job.
```yaml
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
- Now launch the job using the following
```bash
# Create the job
kubectl create -f simple-job.yaml
```
- Check the job
```bash
# Check job status
kubectl get jobs
```
- See which pods are created by the job
```bash
# Check the pods created by the job
kubectl get pods -l job-name=simple-job-<username>
```
- Check the logs for the job
```bash
# View job logs (replace <pod-name> with the actual pod name from above)
kubectl logs <pod-name>
```

### Creating a simple deployment
Deployments manage a set of identical pods and provide rolling updates and rollback capabilities.
```yaml
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
- Now create the deployment
```bash
# Create the deployment
kubectl create -f http-deployment.yaml
```
- Check the deployment
```bash
# Check deployment status
kubectl get deployments
```
- See which pods are created by the deployment
```bash
# Check the pods created by the deployment
kubectl get pods -l k8s-app=test-http
```
On NRP deployments are limited to a `2 week` lifetime.