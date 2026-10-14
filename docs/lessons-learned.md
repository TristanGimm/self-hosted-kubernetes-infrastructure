# Lessons learned

## Prefer the simplest network that meets the need

A mesh VPN like netbird which is a centralized identity can add useful capabilities, but they also add services and failure modes. For this environment, direct WireGuard access was simpler to operate and provided the connectivity that was needed. I overengineered with netbird/

## Troubleshoot networking by layer

VPN routing, host firewall rules, Docker networking, Kubernetes networking, ingress and application behavior interact. Check each boundary separately. The current request path is WireGuard → Caddy → Traefik → Kubernetes Service → Pod.

Monitoring traffic also crosses the host firewall. The node-exporter scrape needs a narrow rule from the internal k3s network even while public access remains blocked.

## Keep the platform within its capacity

A single server can run a useful Kubernetes platform, but it has no node failover. Gitea Actions builds share CPU, memory and disk with production workloads. Add components when their operational benefit justifies their load and maintenance.

## Separate build and deploy responsibilities

Gitea Actions produces an image and updates its desired version in Git. Argo CD applies that version. This makes it easier to identify what was built and what is running.