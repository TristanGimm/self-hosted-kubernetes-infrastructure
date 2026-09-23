# CI Examples

This directory contains simplified examples of CI workflows used to demonstrate build, test and containerization patterns.

Production workflows, credentials, private registries and proprietary application logic are intentionally excluded.

The general workflow is:

1. Source code is pushed to Git
2. CI validates and tests the application
3. A container image is built
4. The image is published to a registry
5. Deployment configuration is updated
6. Argo CD synchronizes the new desired state with Kubernetes
