# Architecture

The platform consists of one Debian 13 server and one k3s node. It is a personal self-hosted setup, not a high-availability cluster.

## Deployed components

| Layer | Components |
|---|---|
| Host access | WireGuard, nftables firewall |
| Host Docker services | Caddy, Vaultwarden, retained legacy PostgreSQL rollback container |
| Kubernetes ingress | Traefik behind Caddy |
| Source and deployment control | Gitea, Actions runner, Argo CD |
| Monitoring | Prometheus, Grafana, Alertmanager, node-exporter, kube-state-metrics |
| Applications | Quant Engine, Trading Dashboard web/worker, Trade Copier |
| Data | Trading PostgreSQL and local-path persistent volumes |

## Source flow

Application code is edited on public GitHub and synchronized into private Gitea by a Gitea Actions workflow. A GitHub webhook dispatches the sync immediately on push; a short repeating schedule runs the same workflow as a fallback for a missed delivery. The sync copies the GitHub working tree into the Gitea repository, keeps the `.gitea` workflow directory out of the copy so the definitions that perform the sync survive, and records the upstream commit SHA next to the source.

The desired state Argo CD watches is not synchronized this way. The private Gitea infrastructure repository is maintained directly and is the only source Argo CD reconciles. This public documentation repository holds no production manifests.

## Request path

```text
Trusted device -> WireGuard -> Caddy (TLS) -> internal host bridge
              -> Traefik ingress -> Kubernetes Service -> Pod
```

WireGuard and Caddy are outside k3s. The VPN and TLS entry point can remain available while Kubernetes is recovered, but services routed to the cluster are unavailable until k3s is healthy.

## Cluster and storage

k3s runs as a single server node with its embedded datastore and containerd. Built-in Traefik and ServiceLB are disabled; a separate Traefik deployment handles Kubernetes ingress. Persistent volumes use local-path storage on the same server disk.

There is no node failover or replicated storage. Adding a second server alone would not provide high availability; datastore quorum, replicated storage, network paths and recovery procedures would also need design.
