# CI/CD Architecture

## Overview

The platform separates Continuous Integration from deployment orchestration.

CI is responsible for validating, building and packaging application changes, while Argo CD handles deployment through GitOps.

## High-Level Flow

```mermaid
flowchart LR
    Dev[Developer]
    Git[Gitea]
    CI[CI Pipeline]
    Registry[Container Registry]
    Config[Deployment Repository]
    Argo[Argo CD]
    K3s[k3s Cluster]

    Dev -->|Push| Git
    Git --> CI
    CI -->|Build & Test| Registry
    CI -->|Update image reference| Config
    Config --> Argo
    Argo --> K3s
