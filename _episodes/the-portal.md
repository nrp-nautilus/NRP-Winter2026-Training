---
title: "The NRP User Portal"
questions:
- "[What is the NRP User Portal?](the-nrp-user-portal)"
- "[What Resources are available?](#resource-availability)"
- "Basic Kubernetes?"
hidden: True

---

## The NRP User portal
The NRP portal can be accessed from [nrp.ai](https://nrp.ai). 
![NRP Portal]({{ page.root }}/fig/thePortal.png)

This is often 

## Resource Availability
Resources available on NRP include:
- CPU (> 31000 cores)
- GPU (> 1400)
- FPGA

Resources are distributed across site known as computing nodes. 
The following dashboard [https://nrp.ai/viz/resources/](https://nrp.ai/viz/resources/) displays details about current status of all NRP resources.
![NRP Dashboard]({{ page.root }}/fig/resourcePage.png)
> ## Notice the taint column!
>
> Taints are labels added to nodes. 
> Taints can be used to indicate a node is reserved for a special purpose or that a node may be marked for maintennance.
> Pods will not land on tainted nodes unless granted special permissions.
{: .callout}



