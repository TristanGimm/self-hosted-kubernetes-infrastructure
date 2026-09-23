# Networking

The platform separates public application access from private administrative access.

## Access Model

```text
Internet
   ↓
Traefik
   ↓
Public Applications

Trusted Devices
   ↓
WireGuard
   ↓
Internal Services
