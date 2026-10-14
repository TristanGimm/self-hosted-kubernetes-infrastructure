# CI example

Application code lives on public GitHub and is synchronized into private Gitea. A GitHub webhook dispatches a `sync-upstream` workflow on push, and a short repeating schedule runs the same workflow as a fallback. The workflow copies the GitHub working tree into the Gitea repository, excluding `.git` and the `.gitea` workflow directory, so the workflow definitions that perform the sync are preserved, and records the upstream commit SHA.

The sync then dispatches `build-images`, which builds an image, publishes it to the private Gitea Container Registry, and updates the image reference in the private GitOps repository. Argo CD applies that desired state.

Two consequences follow from the direction of the copy:

- Application code is edited on GitHub. A direct commit in Gitea is replaced by the next sync.
- Workflow definitions are maintained in Gitea. They are the part GitHub does not own.
