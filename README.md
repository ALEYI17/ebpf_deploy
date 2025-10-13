# 🚀 ebpf_deploy

This repository contains a **Helm chart** to deploy the core backend components of the [InfraSight](https://github.com/ALEYI17/InfraSight) platform on Kubernetes clusters.

InfraSight is a distributed observability system built with **eBPF**, providing low-level system telemetry across your infrastructure.

This chart installs:

- 🛰️ [`ebpf_server`](https://github.com/ALEYI17/ebpf_server): The gRPC backend that receives telemetry and inserts it into the database.
- 🧠 [`InfraSight_sentinel`](https://github.com/ALEYI17/InfraSight_sentinel): Rules engine that analyzes and triggers alerts based on predefined logic.
- 🤖 [`InfraSight_ml`](https://github.com/ALEYI17/InfraSight_ml): Machine learning microservices for anomaly detection (e.g., per-container resource and syscall models).
- 🛢️ ClickHouse: High-performance columnar database used to store eBPF event data.
- 📡 Kafka: Message broker for event streaming between components.

## 📦 Helm Chart Overview

The Helm chart deploys:
- A standalone `ebpf-server` Deployment, along with its ConfigMap, Service, and Secret templates.
- A full **ClickHouse** instance using the **Bitnami** Helm chart.
- An optional **Kafka** broker.
- The **Sentinel** rule engine with configurable rules via ConfigMap.
- **ML microservices** for real-time anomaly detection:
  - `infrasight-ml-resource-per-container`
  - `infrasight-ml-syscall-per-container`
- Kubernetes-native configuration supporting both development and production deployments.

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

## ⚖️ Managing Sentinel Rules

The [`InfraSight_sentinel`](https://github.com/ALEYI17/InfraSight_sentinel) component loads its detection rules from a **ConfigMap**.
You can create your own rule set and reference it in the Helm release.

1. Create a ConfigMap from your rule files:

```bash
kubectl create configmap my-custom-rules --from-file=./rules/
```

2. During Helm installation, pass the name of your ConfigMap:

```bash
helm install infrasight . \
  --set infrasight-sentinel.rules.volume.name=my-custom-rules
```

3. The Sentinel deployment will mount these rules automatically at runtime.

> 💡 Place all your `.yaml` rule definitions in the `./rules/` directory before creating the ConfigMap.



## 📥 Chart Dependencies

This chart depends on the official Bitnami ClickHouse chart:

```yaml
dependencies:
  - name: clickhouse
    version: "9.2.2"
    repository: "oci://registry-1.docker.io/bitnamicharts"
  - name: kafka
    version: "32.4.3"
    repository: "oci://registry-1.docker.io/bitnamicharts"
```
## 📚 Related Repositories

This is part of the **[InfraSight](https://github.com/ALEYI17/InfraSight)** platform:

- [`infrasight-controller`](https://github.com/ALEYI17/infrasight-controller): Kubernetes controller to manage agents
- [`ebpf_loader`](https://github.com/ALEYI17/ebpf_loader): Agent that collects and sends eBPF telemetry from nodes
- [`ebpf_server`](https://github.com/ALEYI17/ebpf_server): Receives and stores events (e.g., to ClickHouse)
- [`ebpf_deploy`](https://github.com/ALEYI17/ebpf_deploy): Helm charts to deploy the stack
- [`InfraSight_ml`](https://github.com/ALEYI17/InfraSight_ml): Machine learning models for anomaly detection.
- [`InfraSight_sentinel`](https://github.com/ALEYI17/InfraSight_sentinel): Rules engine that generates alerts based on predefined detection logic.

