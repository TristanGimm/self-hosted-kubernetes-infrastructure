# Operations

## Health checks

Useful read-only checks on the server:

```sh
kubectl get nodes -o wide
kubectl get pods -A
kubectl -n argocd get applications
kubectl -n monitoring get pods
docker ps
wg show
```

Use Gitea Actions for build logs, Argo CD for sync and health, Grafana for metrics, and Caddy logs for proxy and certificate troubleshooting.

## Recovery boundaries

WireGuard and Caddy are outside k3s, so they can remain available while Kubernetes is recovered. Applications behind Traefik, Gitea, Argo CD and monitoring depend on k3s. This is one node; there is no node to reschedule workloads to if the server fails.

## Persistent data and backups

The default local-path provisioner stores volumes on the server’s disk. This is local persistence, not replication. Back up databases, Gitea data, cluster state and application volumes to storage outside the server, and periodically verify restores.

The retained Docker PostgreSQL container is a migration rollback copy, not the active dashboard database and not an independent backup.

## Configuration and secrets

Argo CD manages declared Kubernetes state from private Gitea. Avoid changing managed resources directly except for recovery, and reconcile Git afterwards. Keep Secret values out of this public repository.
