# CI Examples

This directory contains simplified examples of the CI patterns used in my infrastructure.

The example workflow demonstrates how application source code can be built into an OCI container image and published to GitHub Container Registry.

The general workflow is:

1. Source code is pushed to Gitea.
2. Gitea Actions executes the CI pipeline.
3. The application is built into a container image.
4. The image is published to GHCR.
5. The GitOps configuration references the desired image version.
6. Argo CD synchronizes the desired state into the k3s cluster.

Production credentials, repository URLs and proprietary application code are intentionally excluded.
