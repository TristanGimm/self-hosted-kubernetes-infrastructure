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

## Request path

```text
Trusted device -> WireGuard -> Caddy (TLS) -> internal host bridge
              -> Traefik ingress -> Kubernetes Service -> Pod
```

WireGuard and Caddy are outside k3s. The VPN and TLS entry point can remain available while Kubernetes is recovered, but services routed to the cluster are unavailable until k3s is healthy.

## Cluster and storage

k3s runs as a single server node with its embedded datastore and containerd. Built-in Traefik and ServiceLB are disabled; a separate Traefik deployment handles Kubernetes ingress. Persistent volumes use local-path storage on the same server disk.

There is no node failover or replicated storage. Adding a second server alone would not provide high availability; datastore quorum, replicated storage, network paths and recovery procedures would also need design.
