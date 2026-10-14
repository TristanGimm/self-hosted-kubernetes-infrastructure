# Networking

## Access policy

WireGuard runs on the Debian host. Web services are intended for trusted devices connected to the VPN. The host firewall restricts incoming HTTPS on the public interface while allowing VPN access.

Caddy runs in Docker and terminates TLS. TCP port 80 is reachable for ACME HTTP-01 certificate validation; the catch-all HTTP site returns 404. Application HTTPS remains restricted to VPN clients. Do not open public HTTPS without deliberately changing this policy.

## Proxy path

```text
WireGuard client
  -> Caddy on the host (HTTPS)
  -> internal host/Docker bridge
  -> Traefik in k3s
  -> Kubernetes Ingress
  -> ClusterIP Service
  -> Pod
```

Caddy is the TLS endpoint. Traefik routes Kubernetes traffic behind Caddy. Vaultwarden is routed directly by Caddy to its Docker service and does not run in k3s.

## Firewall and monitoring

The host firewall defaults to restrictive inbound access. Node Exporter port 9100 is allowed only from the internal k3s pod network so Prometheus can collect host metrics; it is not opened to the public internet.

Exact addresses, keys and firewall rules are intentionally excluded from this public repository.
