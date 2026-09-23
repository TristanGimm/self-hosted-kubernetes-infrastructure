# Architecture

The platform runs on a dedicated Debian server with **k3s** as the Kubernetes distribution.

It is organized into four main areas:

| Layer | Components |
|---|---|
| Access | Traefik, WireGuard |
| Platform | Gitea, Argo CD, Vaultwarden |
| Applications | Internal dashboard, Python services |
| Data | PostgreSQL, NATS |

Most workloads run as containerized Kubernetes workloads inside k3s.

The architecture is designed around:

- minimal public exposure
- private administrative access
- declarative deployments
- clear separation between infrastructure and applications
- persistent storage outside application containers

For the visual overview, see the [architecture diagram](../diagrams/architecture.md).
