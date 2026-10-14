# CI/CD

Gitea Actions builds the application images. Argo CD deploys the image versions recorded in the private GitOps repository.

## Current flow

1. Push a change to `main` or manually start the build workflow from a repository’s Actions page.
2. Gitea Actions runs `.gitea/workflows/build-images.yaml` on the self-hosted runner.
3. The workflow builds and publishes an image to the private Gitea Container Registry.
4. It commits the new image reference to the private GitOps repository.
5. Argo CD synchronizes that desired state to k3s.

The Quant Engine, Trading Dashboard and Trade Copier workflows support both push-to-`main` and `workflow_dispatch`. The runner is on the same server as production workloads, so builds share CPU, memory and disk with applications.

## Repository boundaries

The public GitHub repository containing these documents is separate from the private Gitea GitOps repository. It is not automatically mirrored and changes here do not update what Argo CD watches.

There is no Jenkins service or GitHub Container Registry in the current delivery path. Use immutable image tags to trace and roll back deployments.
