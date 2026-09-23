# Architecture & Deployment Diagrams

This directory contains high-level visual documentation of the self-hosted Kubernetes platform.

The diagrams focus on the architecture and deployment workflows of the environment while intentionally omitting sensitive production details such as credentials, private network information, exact firewall rules and proprietary application logic.

## Architecture

[View architecture](architecture.md)

The architecture diagram provides an overview of the platform layers:

- secure public and private access
- dedicated Debian host
- k3s Kubernetes cluster
- platform services
- internal application workloads
- persistent data and messaging

Key technologies include:

- Debian Linux
- k3s
- Traefik
- WireGuard
- Gitea
- Argo CD
- Vaultwarden
- PostgreSQL
- NATS
- Python automation services
- internal dashboard

## CI/CD & GitOps Deployment Flow

[View deployment flow](deployment-flow.md)

The deployment diagram describes how an application change moves from source control to a running workload.

The intended workflow is:

```text
Developer
   ↓
Gitea
   ↓
CI Pipeline
   ├──→ GitHub Container Registry
   │
   └──→ GitOps Configuration
              ↓
           Argo CD
              ↓
             k3s
              ↓
       Running Workload
