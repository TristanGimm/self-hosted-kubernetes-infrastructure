# GitOps

Argo CD continuously reconciles Kubernetes resources from a private Gitea repository.

The Argo CD application `trading-services` watches the `kubernetes/apps` path on the `main` branch of the private infrastructure repository. Automatic sync is enabled. The desired state covers the Quant Engine, Trading Dashboard, PostgreSQL and Trade Copier workloads.

Gitea Actions does not deploy directly to Kubernetes. After publishing an image, the workflow updates its image reference in Git. Argo CD detects that change and reconciles the cluster.

## Public and private copies

The private Gitea infrastructure repository is maintained directly and is the source watched by Argo CD. This public GitHub repository is a documentation copy; it is not synchronized into Gitea, and it deliberately holds no production manifests.

The application repositories work in the opposite direction. Quant Engine, Trading Dashboard and Trade Copier are synchronized from public GitHub into private Gitea, on every GitHub push and on a short repeating schedule as a fallback. See [CI/CD](ci-cd.md).

## Operational guidance

- Change managed desired state in Git and let Argo CD sync it.
- Use the Argo CD UI to inspect sync and health.
- Keep production Secret values out of this public repository.
- Direct `kubectl` changes to managed resources can create drift; update Git afterwards.
