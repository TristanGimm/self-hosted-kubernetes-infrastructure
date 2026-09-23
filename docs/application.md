# Application Workloads

## Overview

The infrastructure hosts several custom internal applications and automation services.

Some of these workloads support proprietary trading and market-data workflows. The business logic itself is private, but the surrounding infrastructure patterns are documented here.

## Python Automation Services

Several Python-based services run as containerized workloads inside the platform.

Their responsibilities include tasks such as:

- collecting external data
- processing internal events
- publishing messages
- storing application data
- communicating with internal services
- providing data to dashboards which i access internally

## Trading-Related Workloads

The platform also hosts proprietary trading-related services.

The public repository intentionally does not include:

- trading algorithms
- execution logic
- broker credentials
- account information
- proprietary strategy code

From an infrastructure perspective, these workloads are treated like other internal applications:

```mermaid
flowchart LR
    External[External Data Sources]
    Service[Python Service]
    NATS[NATS]
    DB[(PostgreSQL)]
    Dashboard[Internal Dashboard]

    External --> Service
    Service --> NATS
    Service --> DB
    NATS --> Dashboard
    DB --> Dashboard
