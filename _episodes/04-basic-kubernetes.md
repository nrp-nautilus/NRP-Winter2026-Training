---
title: "Intro to Kubernetes and Docker"
questions:
hidden: True

---

## Docker and Kubernetes?
Docker is a platform for building, packaging, and running applications in containers.
- Containers are lightweight, portable, and consistent environments.
- They include everything your app needs: code, libraries, and dependencies.
- Docker makes it easy to run the same app on your laptop, a server, or in the cloud.

Why Docker matters for Kubernetes:
- Kubernetes doesn’t build containers, it runs them.
- Docker (or other container runtimes) provides the container images that Kubernetes manages.
- Pod → The smallest deployable unit in Kubernetes, which wraps one or more containers.

Docker Registry
- A repository for storing and sharing Docker images.
- Public example: Docker Hub (hub.docker.com)
- Private registries are possible for internal use.
- NRP’s GitLab: provides public/private registry based on repo’s settings.(If you have a local docker image, can push to gitlab, or build in gitlab)



![NRP Dashboard]({{ page.root }}/fig/NRP-dash.png)




## Where should I ask for help?
The primary option for support is via our [Matrix chat](https://element.nrp-nautilus.io/). 