Deployment Process
1. Code is pushed to the application repository in Gitea.
2. The CI pipeline validates, tests and builds the application.
3. The resulting OCI image is published to GitHub Container Registry.
4. The GitOps configuration is updated with the new image version.
5. Argo CD detects the desired-state change.
6. Argo CD synchronizes the configuration with k3s.
7. Kubernetes pulls the required image from GHCR and starts the updated workload.
This separates application building from deployment:
CI → build and publish
GitOps / Argo CD → deploy and synchronize


Die Gesamtarchitektur erzählt dann sauber:

\[
\text{Internet / Devices}
\rightarrow
\text{Access}
\rightarrow
\text{k3s}
\rightarrow
\text{Platform}
\rightarrow
\text{Applications}
\rightarrow
\text{Data}
\]

und der Deployment-Flow:

\[
\text{Code}
\rightarrow
\text{Gitea}
\rightarrow
\text{CI}
\rightarrow
\text{GHCR}
\rightarrow
\text{GitOps}
\rightarrow
\text{ArgoCD}
\rightarrow
\text{k3s}
\]

Falls **Gitea CI → GHCR → automatisches GitOps-Update** bei dir noch nicht tatsächlich läuft, ändere im zweiten Dokument vorerst `CI Pipeline` zu **`Planned CI Pipeline`**. Dann präsentierst du nichts als produktiv, was noch in Umsetzung ist.
