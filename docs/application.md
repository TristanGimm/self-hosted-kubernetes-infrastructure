# Application workloads

The current applications run in k3s. This repository documents their infrastructure context; it excludes proprietary code, trading logic, broker credentials and account information.

## Trading Dashboard

The Dashboard has a web deployment and a worker deployment. The worker periodically synchronizes the configured Notion data source and updates the active PostgreSQL database in k3s. Notion credentials are stored in a Kubernetes Secret, not in this repository.

The former Docker web and worker containers are stopped. The old Docker PostgreSQL container remains as a rollback copy; it is not the active dashboard database.

## Quant Engine

The Quant Engine runs as a Kubernetes workload and stores token state on a persistent volume. It provides read-only market data and diagnostics and does not submit orders.

## Trade Copier

The Trade Copier runs in k3s and stores its state persistently. Its `ENGINE_TRADING_ENABLED` setting is false, so live trade execution is disabled.

## Monitoring

Prometheus, Grafana and Alertmanager run in k3s. Prometheus scrapes Kubernetes metrics and node-exporter metrics from the Debian host. The Grafana overview shows CPU, memory, root filesystem use, pod state, restarts, unavailable replicas and scrape failures.
