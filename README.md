# Self-Hosted-Kubernetes-Infrastructure
Self-hosted Debian and k3s platform using Argo CD, Traefik, PostgreSQL, NATS, WireGuard and other infrastructure tooling for internal applications and automation.

# Self-Hosted Kubernetes Infrastructure

## Overview
## Architecture
## Core Components
## Deployment & GitOps Workflow
## Networking & Security
## Data & Messaging
## Services
## Engineering Decisions
## Lessons Learned
## Repository Structure
## Disclaimer

## Architecture

```mermaid
flowchart TD
    Internet[Internet] --> Traefik[Traefik Ingress]
    Devices[My Devices] --> WG[WireGuard VPN]

    WG --> Server[Debian Server]
    Traefik --> K3s[k3s Cluster]
    Server --> K3s

    K3s --> ArgoCD[Argo CD]
    K3s --> Gitea[Gitea]
    K3s --> Vaultwarden[Vaultwarden]
    K3s --> Dashboard[Trading Dashboard]
    K3s --> Robots[Python Trading Services]
    K3s --> Postgres[PostgreSQL]
    K3s --> NATS[NATS]

    Gitea --> ArgoCD
    ArgoCD --> Dashboard
    ArgoCD --> Robots
    Dashboard --> Postgres
    Robots --> Postgres
    Robots --> NATS


Das ist erstmal schon sehr gut.

---

## 3. Die Architektur in Blöcke erklären

Nicht einfach Tools auflisten. **Gruppieren**.

Zum Beispiel:

### Core Components

- **Debian Server** as the base operating system
- **k3s** for lightweight Kubernetes orchestration
- **Traefik** for ingress and routing
- **Argo CD** for GitOps-based deployments
- **Gitea** for self-hosted Git services
- **Vaultwarden** for password management
- **PostgreSQL** for application and trading-related data
- **NATS** for messaging between services
- **WireGuard** for secure access across devices
- **Python services** for trading automation and data collection
- **Custom dashboard** for internal visualization and management

---

## 4. Die echten technischen Entscheidungen zeigen

Das ist extrem wichtig.

Recruiter lieben nicht nur „ich habe X benutzt“, sondern:

\[
\boxed{\text{Ich habe X getestet, Problem erkannt, dann Y gewählt}}
\]

Zum Beispiel mit NetBird:

### Engineering Decisions

```md
## Engineering Decisions

### VPN / Private Network Access
I initially evaluated NetBird as a mesh VPN solution for secure device connectivity.  
While the setup worked correctly, I observed latency issues on iPhone and iPad clients.  
I therefore moved to a WireGuard-based setup for more predictable performance and lower overhead.

### GitOps
I chose Argo CD to manage Kubernetes deployments declaratively and reduce manual deployment steps. This improved consistency and made it easier to reason about changes across services.

### Messaging
NATS is used for lightweight internal messaging between services, especially for trading-related automation where decoupled communication is useful.

### Data Layer
PostgreSQL is used as the main persistent data store for application and trading-related information.

## Lessons Learned

- Operating a self-hosted platform requires careful attention to networking, storage and service reliability.
- VPN and private networking solutions should be evaluated not only for security, but also for latency and usability across devices.
- GitOps significantly improves reproducibility and reduces operational friction.
- Even a single-node infrastructure environment can be a strong platform for learning production-like operational workflows.
