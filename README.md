# Self-Hosted Kubernetes Infrastructure

Documentation for a self-hosted platform running on one Debian server. The current setup uses k3s, Gitea Actions and Argo CD for the trading applications, with WireGuard-only access to web services.

> This public repository contains documentation and sanitized examples. Production manifests and secrets are kept separately. The same named repository in private Gitea is a separate copy used by Argo CD; it is not automatically synchronized with this GitHub repository.

## Current architecture

- One Debian 13 server runs a single-node k3s cluster.
- WireGuard runs on the host.
- Caddy runs in Docker on the host, terminates TLS and forwards app traffic to Traefik inside k3s.
- Gitea, its Actions runner, Argo CD, Prometheus, Grafana and Alertmanager run in k3s.
- The Quant Engine, Trading Dashboard web and Notion-sync worker, Trade Copier, and dashboard PostgreSQL run in k3s.
- Vaultwarden remains in Docker outside k3s. A legacy trading PostgreSQL container remains as a rollback copy.
- Persistent volumes use local-path storage on the server disk; there is no replicated storage.
- The Trade Copier is deployed with live trading disabled. Robots and backtesting workers are future workloads. NATS is not deployed.

## Architecture and delivery

- [Platform architecture](diagrams/architecture.md)
- [CI/CD deployment flow](diagrams/deployment-flow.md)

Application workflows build images into the private Gitea Container Registry and update the private GitOps repository. Argo CD syncs that desired state into k3s.

## Documentation

- [Architecture](docs/architecture.md)
- [Networking and VPN](docs/networking.md)
- [GitOps](docs/gitops.md)
- [CI/CD](docs/ci-cd.md)
- [Applications](docs/application.md)
- [Operations](docs/operations.md)
- [Lessons learned](docs/lessons-learned.md)
- [Examples](examples/)

## Operational limits

A host, disk or k3s failure can interrupt every Kubernetes workload. Local-path volumes do not provide replication or node failover. WireGuard and Caddy run outside k3s, so the VPN and TLS entry point do not depend on the cluster starting; services behind Traefik do.

This repository excludes credentials, tokens, VPN keys, private addresses, production manifests, account details and proprietary trading logic.
