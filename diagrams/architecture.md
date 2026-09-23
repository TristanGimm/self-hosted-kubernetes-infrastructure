# Architecture

The diagram below shows the high-level architecture of the self-hosted platform.  
It is intentionally simplified and excludes sensitive production details.

Architecture Layers
Access Layer
- Traefik handles ingress and routing for selected externally reachable services.
- WireGuard provides secure access to private and administrative services.
Platform Services
- Gitea provides self-hosted source control.
- Argo CD synchronizes Git-managed application state with Kubernetes.
- Vaultwarden provides self-hosted password management.
Application Workloads
- Python Automation Services run internal automation and trading-related workloads.
- Internal Dashboard provides visualization and management of internal data.
Data & Messaging
- PostgreSQL provides persistent relational storage.
- NATS provides lightweight asynchronous messaging between services.

```mermaid
flowchart TB

    Internet((Internet))
    Devices[Trusted Devices]

    subgraph Access["Access Layer"]
        Traefik[Traefik Ingress]
        WireGuard[WireGuard VPN]
    end

    subgraph Host["Dedicated Debian Host"]

        subgraph K3s["k3s Kubernetes Cluster"]

            subgraph Platform["Platform Services"]
                Gitea[Gitea]
                ArgoCD[Argo CD]
                Vaultwarden[Vaultwarden]
            end

            subgraph Applications["Application Workloads"]
                Dashboard[Internal Dashboard]
                Robots[Python Automation Services]
            end

            subgraph Data["Data & Messaging"]
                PostgreSQL[(PostgreSQL)]
                NATS[NATS]
            end

        end

    end

    Internet --> Traefik
    Devices --> WireGuard

    Traefik --> Dashboard
    Traefik --> Vaultwarden

    WireGuard --> Gitea
    WireGuard --> ArgoCD
    WireGuard --> Dashboard

    Gitea -->|Desired state| ArgoCD

    ArgoCD -->|GitOps deployment| Dashboard
    ArgoCD -->|GitOps deployment| Robots

    Robots -->|Persistent data| PostgreSQL
    Robots -->|Events| NATS

    Dashboard -->|Read / write| PostgreSQL
    Dashboard -->|Events| NATS
