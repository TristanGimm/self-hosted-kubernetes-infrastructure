# Argo CD Examples

This directory contains examples demonstrating the GitOps deployment patterns used in my platform.

Argo CD is used to synchronize the desired application state stored in Git with the corresponding k3s cluster. It is well suited for my use case because it automates the deployment process and eliminates the need to manually repeat deployment steps.

When I make changes to the code of my robots, I only need to push those changes to the Git repository. Argo CD then detects the updated desired state and automatically synchronizes it with the Kubernetes cluster. This makes deployments more consistent, reproducible, and significantly reduces the amount of manual work required.

The examples in this directory are intentionally simplified and do not expose private repositories, credentials, secrets, or production-specific configuration.
