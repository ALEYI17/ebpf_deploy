# 🚀 ebpf_deploy

This repository contains a **Helm chart** to deploy the core backend components of the [InfraSight](https://github.com/ALEYI17/InfraSight) platform on Kubernetes clusters.

InfraSight is a distributed observability system built with **eBPF**, providing low-level system telemetry across your infrastructure.

This chart installs:

- 🛰️ [`ebpf_server`](https://github.com/ALEYI17/ebpf_server): The gRPC backend that receives telemetry and inserts it into the database.
- 🛢️ ClickHouse: High-performance columnar database used to store eBPF event data.

## 📦 Helm Chart Overview

The Helm chart deploys:

- A standalone `ebpf-server` Deployment, along with its ConfigMap, Service, and Secret templates
- A full ClickHouse instance using the **Bitnami** Helm chart (`oci://registry-1.docker.io/bitnamicharts`)
- Kubernetes-native configuration to support both development and production deployments

## 📌 Status

> 🧪 **Currently experimental.**
>
> This chart is primarily designed for **development** and **testing environments**. It is **not yet published** to a Helm registry and must be installed locally.

## ⚙️ Dependencies

- [Helm](https://helm.sh/) 3.0+
- Kubernetes cluster (local or cloud)
- Docker (if testing locally with KIND or Minikube)

## 🧪 Local Installation

To deploy this chart in a development cluster:

```bash
git clone https://github.com/ALEYI17/ebpf_deploy.git
cd ebpf_deploy

helm dependency update  # fetch Bitnami ClickHouse chart

helm install infrasight .  # deploy with release name "infrasight"
````

This will install:

* `ebpf-server` listening on its configured gRPC port (default: `8080`)
* A ClickHouse instance with default user and password

## 📥 Chart Dependencies

This chart depends on the official Bitnami ClickHouse chart:

```yaml
dependencies:
  - name: clickhouse
    version: "9.2.2"
    repository: "oci://registry-1.docker.io/bitnamicharts"
```
## 📚 Related Repositories

This is part of the **[InfraSight](https://github.com/ALEYI17/InfraSight)** platform:

- [`infrasight-controller`](https://github.com/ALEYI17/infrasight-controller): Kubernetes controller to manage agents
- [`ebpf_loader`](https://github.com/ALEYI17/ebpf_loader): Agent that collects and sends eBPF telemetry from nodes
- [`ebpf_server`](https://github.com/ALEYI17/ebpf_server): Receives and stores events (e.g., to ClickHouse)
- [`ebpf_deploy`](https://github.com/ALEYI17/ebpf_deploy): Helm charts to deploy the stack
