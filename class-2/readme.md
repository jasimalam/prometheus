
# Prometheus Monitoring Setup

This repository contains configuration files, documentation, and command references for setting up Prometheus monitoring in both Docker and Kubernetes environments.

## Repository Structure

| File / Folder                                | Description                                                                                       |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| [`docker-compose.yml`](./docker-compose.yml) | Docker Compose configuration to spin up Prometheus with volume mounts and service definitions.    |
| [`prometheus.yml`](./prometheus.yml)         | Prometheus configuration file defining scrape jobs, global settings, and rule files.              |
| [`file_sd_targets/`](./file_sd_targets)      | Folder for file-based service discovery (file\_sd). Useful for dynamically adding scrape targets. |
| [`k8s-deploy.md`](./k8s-deploy.md)           | Instructions for deploying Prometheus to Kubernetes using Helm and manifest files.                |
| [`promql-commands.md`](./promql-commands.md) | Practical PromQL command examples for learning and operational use.                               |
| [`readme.md`](./readme.md)                   | This file. Provides an overview and navigation for the repository.                                |

## Getting Started

### Docker Setup

```bash
docker-compose up -d
```

This launches Prometheus using the config from [`prometheus.yml`](./prometheus.yml). The service runs with data persistence enabled via volumes.

### Kubernetes Setup

Refer to [`k8s-deploy.md`](./k8s-deploy.md) for complete instructions on deploying Prometheus on a Kubernetes cluster using `kube-prometheus-stack` or manually with manifests.

### PromQL Examples

See [`promql-commands.md`](./promql-commands.md) for PromQL commands

### Dynamic Service Discovery

Prometheus uses the [`file_sd_targets/`](./file_sd_targets) directory for dyanmic targets
Edit or add new `.json` target files without restarting Prometheus.

## Prerequisites

* Docker and Docker Compose (for local setup)
* A Kubernetes cluster and `kubectl` access
* Helm (for Kubernetes Helm deployments)
* Exporters like Node Exporter or cAdvisor

## References

* [Prometheus Documentation](https://prometheus.io/docs/)
* [PromQL Cheat Sheet](https://promlabs.com/promql-cheat-sheet/)
* [kube-prometheus-stack Helm Chart](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack)
* [Exporter and Integration](https://prometheus.io/docs/instrumenting/exporters/)