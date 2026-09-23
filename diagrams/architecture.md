flowchart TB

    %% ACCESS
    subgraph ACCESS["ACCESS"]
        direction LR
        Internet((Internet))
        Traefik[Traefik]
        Devices[Trusted Devices]
        WireGuard[WireGuard]

        Internet --> Traefik
        Devices --> WireGuard
    end

    %% INFRASTRUCTURE
    subgraph HOST["DEBIAN SERVER"]
        direction TB

        subgraph CLUSTER["k3s CLUSTER"]
            direction TB

            subgraph PLATFORM["PLATFORM"]
                direction LR
                Gitea[Gitea]
                ArgoCD[Argo CD]
                Vaultwarden[Vaultwarden]
            end

            subgraph APPS["APPLICATIONS"]
                direction LR
                Dashboard[Internal Dashboard]
                Automation[Python Automation Services]
            end

            subgraph DATA["DATA & MESSAGING"]
                direction LR
                PostgreSQL[(PostgreSQL)]
                NATS[NATS]
            end
        end
    end

    %% EXTERNAL ACCESS
    Traefik --> Dashboard
    WireGuard --> ArgoCD

    %% PLATFORM FLOW
    Gitea --> ArgoCD

    %% DEPLOYMENT
    ArgoCD --> Dashboard
    ArgoCD --> Automation

    %% APPLICATION DATA
    Dashboard --> PostgreSQL
    Automation --> PostgreSQL
    Automation --> NATS

    %% LAYOUT HELPERS
    Gitea ~~~ Vaultwarden
    Dashboard ~~~ Automation
    PostgreSQL ~~~ NATS
