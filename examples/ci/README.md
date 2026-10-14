# CI example

Current application repositories use Gitea Actions. A push to `main` or manual workflow dispatch builds an image, publishes it to the private Gitea Container Registry, and updates the image reference in the private GitOps repository. Argo CD applies that desired state.
