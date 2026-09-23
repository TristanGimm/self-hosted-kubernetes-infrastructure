# Architecture

This document describes the high-level architecture of my self-hosted Kubernetes platform.

The diagram intentionally focuses on the major infrastructure layers and omits sensitive production details such as private addresses, credentials, firewall rules and proprietary application logic.

## High-Level Architecture

```mermaid
flowchart TB

    %% ─────────────────────────────────────────────
    %% ACCESS
    %% ─────────────────────────────────────────────

    subgraph Access["Access Layer"]
        direction LR
        Internet((Internet))
        Devices[Trusted Devices]
        Traefik[Traefik Ingress]
        WireGuard[WireGuard VPN]

        Internet --> Traefik
        Devices --> WireGuard
    end


    %% ─────────────────────────────────────────────
    %% HOST / KUBERNETES
    %% ─────────────────────────────────────────────

    subgraph Host["Dedicated Debian Host"]

        subgraph Cluster["k3s Kubernetes Cluster"]

            subgraph Platform["Platform Services"]
                direction LR
                Gitea[Gitea]
                ArgoCD[Argo CD]
                Vaultwarden[Vaultwarden]
            end

            subgraph Applications["Application Workloads"]
                direction LR
                Dashboard[Internal Dashboard]
                Automation[Python Automation Services]
            end

            subgraph Data["Data & Messaging"]
                direction LR
                PostgreSQL[(PostgreSQL)]
                NATS[NATS]
            end

        end
    end


    %% ─────────────────────────────────────────────
    %% ACCESS PATHS
    %% ─────────────────────────────────────────────

    Traefik --> Dashboard

    WireGuard --> Gitea
    WireGuard --> ArgoCD
    WireGuard --> Vaultwarden


    %% ─────────────────────────────────────────────
    %% GITOPS
    %% ─────────────────────────────────────────────

    Gitea --> ArgoCD

    ArgoCD --> Dashboard
    ArgoCD --> Automation


    %% ─────────────────────────────────────────────
    %% APPLICATION DATA FLOW
    %% ─────────────────────────────────────────────

    Dashboard --> PostgreSQL

    Automation --> PostgreSQL
    Automation --> NATS
```

---

## Architecture Layers

| Layer | Components | Responsibility |
|---|---|---|
| **Access** | Traefik, WireGuard | Public ingress and secure private access |
| **Platform** | Gitea, Argo CD, Vaultwarden | Source control, GitOps and internal platform services |
| **Applications** | Dashboard, Python services | Internal applications and automation workloads |
| **Data & Messaging** | PostgreSQL, NATS | Persistent storage and asynchronous communication |

---

## Access Layer

### Traefik

Traefik provides ingress and routing for selected externally reachable applications.

Only services that require public access are exposed through the ingress layer.

### WireGuard

WireGuard provides secure private connectivity for trusted devices.

Administrative services such as Gitea and Argo CD remain accessible through the private network instead of being unnecessarily exposed to the public internet.

---

## Platform Services

### Gitea

Gitea provides self-hosted Git repositories for application and infrastructure configuration.

### Argo CD

Argo CD implements the GitOps deployment model.

Git contains the desired application state, while Argo CD synchronizes that state with the k3s cluster.

### Vaultwarden

Vaultwarden provides self-hosted password management and is treated as an internal service.

---

## Application Workloads

### Internal Dashboard

The dashboard provides visualization and management of internal application and operational data.

### Python Automation Services

Containerized Python services handle internal automation, data collection and trading-related workloads.

The proprietary business logic of these applications is intentionally excluded from this repository.

---

## Data & Messaging

### PostgreSQL

PostgreSQL provides persistent relational storage for internal applications and automation services.

### NATS

NATS provides lightweight asynchronous messaging for event-driven communication between services.

---

## Design Principles

The platform follows several basic principles:

- **Git as source of truth** for deployable application state
- **Private by default** for administrative services
- **Minimal public exposure**
- **Declarative deployments** through Argo CD
- **Separation of platform, applications and data**
- **Automation of repetitive operational tasks**
- **Simple infrastructure where additional complexity provides no clear benefit**

---

## Related Documentation

- [CI/CD & Deployment Flow](deployment-flow.md)
- [Networking & Security](../docs/networking.md)
- [GitOps](../docs/gitops.md)
- [Operations & Troubleshooting](../docs/operations.md)
