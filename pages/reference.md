---
layout: reference
---

## Glossary

{:auto_ids}
NRP (National Research Platform)
:   A distributed Kubernetes cluster partnership of more than 70 institutions, providing researchers with access to high-performance computing resources across multiple locations.

Nautilus
:   The primary Kubernetes distribution cluster operated by NRP, with hardware spanning across 3 continents.

Namespace
:   A logical grouping mechanism in Kubernetes that organizes users and resources. In NRP, users are organized into namespaces, with members being either namespace `users` or namespace `admins`.

Namespace Admin
:   A user role in NRP that has the ability to create multiple namespaces, invite other users to their namespace(s), and manage namespace resources. Admins are responsible for all activity happening in their namespaces.

Authentik
:   The authentication system used by NRP for user access and identity management.

kubectl
:   The Kubernetes command-line tool used to interact with Kubernetes clusters. Required for NRP training exercises.

kubelogin
:   A kubectl plugin required for OIDC (OpenID Connect) authentication with NRP. Must be installed for the kubeconfig file to work.

Pod
:   The smallest deployable unit in Kubernetes, which wraps one or more containers. Pods run on worker nodes and are managed by the Kubernetes control plane.

Deployment
:   A Kubernetes resource that manages a set of identical pods, ensuring a specified number of replicas are running at all times.

Service
:   A Kubernetes resource that provides a stable network endpoint to access a set of pods, abstracting away the individual pod IP addresses.

Persistent Volume Claim (PVC)
:   A request for storage by a user. In NRP, different storage classes are available including `rook-ceph-block` and `rook-cephfs`.

JupyterHub
:   A multi-user server for Jupyter notebooks that allows users to launch and use interactive development environments on NRP.

Coder
:   A development environment platform that provides VS Code (code-server) support, allowing users to launch interactive development environments on NRP.

Helm
:   A package manager for Kubernetes that simplifies the deployment and management of applications using charts.

S3 Storage
:   Object storage service available on NRP for storing and retrieving data. Access requires S3 credentials (access key and secret key).

Node Affinity
:   A Kubernetes feature that allows you to constrain which nodes your pod can be scheduled on based on node labels, such as GPU type or region.

Taint and Toleration
:   Kubernetes mechanisms for node selection. Taints allow a node to repel a set of pods, while tolerations allow pods to be scheduled on tainted nodes.

Priority Class
:   A Kubernetes resource that defines the relative priority of pods. In NRP, the `opportunistic` priority class is available for lower-priority workloads.

{% include links.md %}
