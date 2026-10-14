# Networking examples

In the deployed setup, WireGuard and Caddy stay on Debian. Caddy terminates TLS and sends requests to Traefik in k3s. Access to Node Exporter is limited to the internal k3s network.
