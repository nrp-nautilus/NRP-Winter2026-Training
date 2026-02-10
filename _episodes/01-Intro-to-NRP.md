---
title: "Intro to NRP"
questions:
- "How do I get started with NRP?"
- "[Where to ask for help?](#where-should-i-ask-for-help)"
- "Basic Kubernetes?"
hidden: True

---

## What is NRP?
<!--Check these numbers-->
- The National Research Platform is a partnership of more than 70 institutions.
- Includes contributions by the National Science Foundation, the Department of Energy, the Department of Defense,  and many research universities and R&E networking organizations in the US and around the world.
- NRP operates a primary Kubernetes distribution cluster with hardware spanning across 3 continents, called Nautilus. 
- NRP has been in continuous operation for 6 years.
![NRP Dashboard]({{ page.root }}/fig/NRP-dash.png)

- Led by researchers at **UC San Diego**, **University of Nebraska-Lincoln**, and **MGHPCC**.
![NRP]({{ page.root }}/fig/NRP.jpg)

### NRP is a Kubernetes Cluster
- **Control plane**: Manages the cluster and the worker nodes
- **Worker Nodes**: Maintain running pods and provide Kubernetes runtime environment
- Various storage options like **CephFS**; **CvmFS**; **S3**. 
- Advanced monitoring tools like **PerfSONAR**; **traceroute**; **Prometheus**.
- Diverse computation and data tools: **JupyterHub**; **WebODM**; **GitLab**; **Nextcloud**; **Overleaf**.
- Collaboration and communication: **Jitsi**; **EtherPad**; **Hedgedoc**; **SyncThing**.


<img src="{{ page.root }}/fig/kcluster.png" alt="Kubernetes Cluster" width="500">

### Hardware on NRP[^gpu-types]
- 430+ nodes 
- 1400+ GPUs 
- 50+ FPGAs 
- Bluefield 2 SmartNICs 
- SmartSSDs
- programmable Tofino switches
<div style="display:flex; gap:16px; align-items:flex-start; flex-wrap:wrap;">
  <img src="{{ page.root }}/fig/GPU-pie.png" alt="GPU Pie Chart" style="width:48%; max-width:480px; height:auto;">
  <img src="{{ page.root }}/fig/GPUModels.png" alt="GPU List" style="width:48%; max-width:480px; height:auto;">
</div>
[^gpu-types]: As of Feb 2026

### Storage Options on NRP
You will learn that Kubernetes runs your workload in objects called `Pods`, and that Pods are ephemeral by nature. To keep data beyond the lifetime of a Pod, Kubernetes provides persistent storage via `PersistentVolume` resources, which Pods mount using a `PersistentVolumeClaim`. NRP offers a variety of storage options to suit different workload and performance needs.
![NRP]({{ page.root }}/fig/storage.png)
- Ceph File System ➡ Ceph FS ➡ ReadWriteMany PersistentVolumes
- Ceph Rados Block Device ➡ Ceph RBD ➡ ReadWriteOnly PersistentVolumes
- Linstor Block ➡ ReadWriteOnly PersistentVolumes
- S3 Compatible storage ➡ Bucket Endpoints
- Open Science Data Federation ➡ OSDF


## How do I get started?
A full guide to getting started can be found in our [documentation](https://nrp.ai/documentation/userdocs/start/getting-started/). 

The first step is to visit [nrp.ai](https://nrp.ai/) and log in at the upper right-hand corner. This should open a **CILogon** sign-in page.
![CILogon]({{ page.root }}/fig/CILogon.png)
> ## Choosing an Identity Provider
>
> We recommend that you select your **institution** as the identity provider (IdP) in the CILogon page. 
> Other options like google, Microsoft, and ORCID exist, but may face limitations on NRP
> If you used the wrong IdP and would like to change it, let NRP admins know and we can help.
{: .callout}

## Where should I ask for help?
The primary option for support is via our [Matrix chat](https://element.nrp-nautilus.io/). Please use public channels when seeking support, this allows the whole NRP community to offer advice. NRP admins actively monitor and respond to the **NRP Support** channel in Matrix. 
<div style="display:flex; gap:16px; align-items:flex-start; flex-wrap:wrap;">
  <img src="{{ page.root }}/fig/matrixSupport.png" alt="Support" style="width:48%; max-width:480px; height:auto;">
  <img src="{{ page.root }}/fig/matrixNews.png" alt="News" style="width:48%; max-width:480px; height:auto;">
</div>
- NRP hosts a Matrix Server. For info about joining: [https://nrp.ai/contact/](https://nrp.ai/contact/)
- We provide user support in **Nautilus Support**
- We announce news in **NRP News**
- Element Client is free cross-platform, we host a web version at: [https://element.nrp-nautilus.io/](https://element.nrp-nautilus.io/)

## Let's Get Started
![Top Uses]({{ page.root }}/fig/TopUse.png)

The majority of NRP users interact with the cluster using the following three methods.
- via **Kubernetes**: Directly submit and manage containerized workloads (services and batch jobs) using Kubernetes APIs and tools like `kubectl`.
- via the **Coder** service: Launch a browser-based VS Code environment connected to cluster resources for interactive development and execution.
- via NRP deployed **Jupyterhub**: Start a JupyterLab notebook server on the cluster for interactive analysis, prototyping, and teaching workflows.

In order to access any of these services, users must first be added as a member of a namespace.

<hr style="border:0; border-top:2px solid #8f8e8e; margin:2rem 0;">