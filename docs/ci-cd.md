# CI/CD

Gitea Actions builds the application images. Argo CD deploys the image versions recorded in the private GitOps repository.

## Current flow

1. Push a change to `main` on the public GitHub repository, or manually start the build workflow from a repository’s Actions page.
2. A GitHub webhook dispatches the sync workflow in Gitea immediately. A short repeating schedule runs the same workflow as a safety net if a webhook delivery is missed.
3. Gitea Actions runs `.gitea/workflows/sync-upstream.yaml`, which copies the GitHub working tree into the matching private Gitea repository.
4. The sync dispatches `build-images` explicitly, because Gitea does not trigger workflows for pushes made by Actions itself.
5. Gitea Actions runs `.gitea/workflows/build-images.yaml` on the self-hosted runner.
6. The workflow builds and publishes an image to the private Gitea Container Registry.
7. It commits the new image reference to the private GitOps repository.
8. Argo CD synchronizes that desired state to k3s.

The Quant Engine, Trading Dashboard and Trade Copier repositories are synchronized this way and support both push-to-`main` and `workflow_dispatch` builds. The runner is on the same server as production workloads, so builds share CPU, memory and disk with applications.

## Source synchronization

GitHub is the editing surface for application code. The sync workflow copies the GitHub working tree into Gitea and records the upstream commit SHA next to it, so the built revision stays traceable to a commit on GitHub.

The copy excludes two paths: `.git`, and `.gitea`, which holds the workflow definitions. Excluding `.gitea` is what keeps the sync from overwriting the workflows that perform it.

Because the sync rewrites the application files on every run, two rules follow:

- Do not commit application code directly in Gitea. The next sync replaces it with the GitHub state.
- Maintain workflow definitions in the private Gitea repository. They are the part GitHub does not own.

## Repository boundaries

The documentation and GitOps repository is deliberately not synchronized. This public GitHub repository is a documentation copy, and the private Gitea repository of the same name is maintained directly and carries the `kubernetes/apps` manifests Argo CD watches. Changes to the documentation here do not update what Argo CD watches.

There is no Jenkins service or GitHub Container Registry in the current delivery path. Use immutable image tags to trace and roll back deployments.
