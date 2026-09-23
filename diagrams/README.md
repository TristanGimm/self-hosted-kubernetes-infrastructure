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
- Python automation and trading-related services

### `deployment-flow.png`
Shows the CI/CD and GitOps deployment flow, including:

- source code management in Gitea
- CI pipeline execution
- container image publishing
- GitOps-based deployment updates
- Argo CD synchronization
- deployment into the k3s cluster

## Notes

These diagrams are intentionally simplified.

They do not include:

- private IP addresses
- credentials or secrets
- internal domains
- exact firewall rules
- proprietary application logic
- broker integrations
- trading strategy internals
