# GitOps

Argo CD continuously reconciles Kubernetes resources from a private Gitea repository.

The Argo CD application `trading-services` watches the `kubernetes/apps` path on the `main` branch of the private infrastructure repository. Automatic sync is enabled. The desired state covers the Quant Engine, Trading Dashboard, PostgreSQL and Trade Copier workloads.

Gitea Actions does not deploy directly to Kubernetes. After publishing an image, the workflow updates its image reference in Git. Argo CD detects that change and reconciles the cluster.

## Public and private copies

This public GitHub repository is a separate documentation copy. It is not the source watched by Argo CD and is not automatically synchronized with the private Gitea repository.

## Operational guidance

- Change managed desired state in Git and let Argo CD sync it.
- Use the Argo CD UI to inspect sync and health.
- Keep production Secret values out of this public repository.
- Direct `kubectl` changes to managed resources can create drift; update Git afterwards.
