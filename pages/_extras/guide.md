---
title: "Instructor Notes"
---

## Training Overview

This training is designed as two 3-hour sessions:
- **Beginner Track** (3 hours): Introduction to NRP, portal usage, basic Kubernetes
- **Intermediate Track** (3 hours): JupyterHub, Coder, deploying multi-tenant environments

## Prerequisites for Learners

Before the training, learners should:
1. Have NRP access via Authentik (institutional account)
2. Be part of at least one namespace (check at [https://nrp.ai/namespaces/](https://nrp.ai/namespaces/))
3. Have `kubectl` and `kubelogin` installed (instructions in Beginner Track notebook)
4. Have downloaded the Kubernetes config file to `~/.kube/config`

## Training Environment

### Live Training
- Conducted via Zoom with integrated NRP JupyterHub access
- Learners use `jupyterhub-west.nrp-nautilus.io` as their local environment
- All commands can be copy-pasted from the notebooks

### Self-Paced Learning
- Learners can use their local environment with `kubectl`
- Full setup instructions: [https://nrp.ai/documentation/userdocs/start/getting-started/](https://nrp.ai/documentation/userdocs/start/getting-started/)

## Lesson Design

### Beginner Track (3 hours)
1. **Introduction to NRP** (30 min)
   - What is NRP, mission, capabilities
   - Access requirements and policies
   - Getting help resources

2. **The Portal** (30 min)
   - Navigating the portal
   - Namespace and group management
   - Resource availability

3. **Basic Kubernetes** (2 hours)
   - Installing and configuring kubectl
   - Pods, Deployments, Services
   - Persistent Volume Claims
   - S3 storage access
   - Node affinity and scheduling

### Intermediate Track (3 hours)
1. **Using JupyterHub** (45 min)
   - Launching and using JupyterHub
   - Image profiles and resource limits
   - Managing notebooks

2. **Using Coder** (45 min)
   - Launching and using Coder workspaces
   - Git integration
   - Workspace management

3. **Deploying JupyterHub** (90 min)
   - Helm charts and deployment
   - Configuration with Helm values
   - Multi-tenant environments
   - Troubleshooting

## Technical Tips

### For Instructors
- Use screen sharing with large font sizes (18-20pt)
- Have the notebooks open in JupyterHub for live coding
- Test all commands before the session
- Have backup solutions ready for common issues

### Common Setup Issues
- **kubectl not found**: Ensure PATH includes kubectl installation directory
- **kubelogin errors**: Check token cache settings, browser command, listen port
- **Authentication failures**: Verify config file is in `~/.kube/config`
- **Namespace access denied**: Confirm user is part of namespace at [https://nrp.ai/namespaces/](https://nrp.ai/namespaces/)

## Common Problems

### Learners Can't Access Namespace
- Verify they're part of a namespace: [https://nrp.ai/namespaces/](https://nrp.ai/namespaces/)
- Students: Contact research supervisor to be added
- Faculty/Researchers: Request admin status in Matrix

### kubectl Authentication Issues
- Ensure `kubelogin` plugin is installed
- Check config file location: `~/.kube/config`
- Try cross-platform fixes (token cache, browser command, listen port)

### JupyterHub Not Starting
- Check namespace resource quotas
- Verify image profile is available
- Check pod logs: `kubectl logs -n <namespace> <pod-name>`

### Intermediate Track: Admin Requirements
- Remind learners that deploying JupyterHub requires namespace admin status
- Non-admins can still follow along but won't be able to deploy

## Support Resources

- **Matrix**: [https://nrp.ai/contact/](https://nrp.ai/contact/)
- **Email**: usersupport@nrp-nautilus.io
- **Documentation**: [https://nrp.ai/documentation/](https://nrp.ai/documentation/)
