# GitOps

Argo CD is used to manage Kubernetes deployments declaratively.

Instead of manually changing workloads inside the cluster, the desired application state is stored in Git.

The basic workflow is:

```text
Git
 ↓
Argo CD
 ↓
k3s
 ↓
Application
