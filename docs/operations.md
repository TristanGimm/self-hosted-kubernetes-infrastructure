# Operations

Running the platform involves maintaining the infrastructure beyond the initial deployment.

Typical operational work includes:

- Kubernetes deployment troubleshooting
- Linux administration
- service connectivity debugging
- VPN maintenance
- ingress configuration
- persistent storage
- application updates
- routing and firewall troubleshooting

## Troubleshooting Approach

I generally isolate problems by layer:

```text
Application
    ↓
Pod / Deployment
    ↓
Service
    ↓
Ingress
    ↓
Cluster Network
    ↓
Host Network
    ↓
External / VPN Access
