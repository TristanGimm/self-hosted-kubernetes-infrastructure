# Architecture Diagrams

This directory contains simplified visual documentation of the self-hosted Kubernetes platform.

The diagrams are intended to provide a high-level understanding of the infrastructure, deployment flow and platform design without exposing sensitive production details.

## Included Diagrams

### `architecture.png`
Shows the high-level platform architecture, including:

- Debian host
- k3s cluster
- Traefik ingress
- WireGuard private access
- Argo CD
- Gitea
- Vaultwarden
- PostgreSQL
- NATS
- internal dashboard 
- Python automation and trading-related copier and robots

### `deployment-flow.png`
Shows the CI/CD and GitOps deployment flow, including:

- source code management in Gitea
- CI pipeline execution
- container image publishing
- GitOps-based deployment updates
- Argo CD synchronization
- deployment into the k3s cluster
