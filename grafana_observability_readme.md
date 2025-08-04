# Grafana Observability Stack on Kubernetes

This repository provides a complete, production-ready observability stack for Kubernetes using the **Grafana ecosystem**, including:

- [Prometheus](https://prometheus.io/) for metrics
- [Loki](https://grafana.com/oss/loki/) for logs
- [Tempo](https://grafana.com/oss/tempo/) for traces
- [Pyroscope](https://grafana.com/oss/pyroscope/) for continuous profiling
- [Grafana](https://grafana.com/) as a unified visualization layer

Each component is configured using Helm and deployed in the `monitoring` namespace. All values files provided are ready to be deployed.

## 🔧 Components & Configuration

### 1. Prometheus

- Collects and scrapes metrics from your Kubernetes workloads.
- Preconfigured Helm values file included.

### 2. Loki + Promtail

- Loki ingests logs, and Promtail scrapes logs from pods and ships them to Loki.
- SingleBinary Loki deployment with MinIO as an object store backend.

### 3. Tempo

- Lightweight distributed tracing backend.
- Supports OTLP, Jaeger, and Zipkin formats.

### 4. Pyroscope

- Continuous profiling integration for Go, Python, and other runtimes.
- Visualizes flamegraphs in Grafana alongside traces and logs.

### 5. Grafana

- Preconfigured with datasources for Prometheus, Loki, Tempo, and Pyroscope.
- Persistent volume configured.

## 🚀 Quick Start

> Ensure you have Helm and `kubectl` set up and are pointing to your cluster.

```bash
kubectl create namespace monitoring

# Add Grafana Helm repo
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

# Install Prometheus
helm upgrade --install prometheus grafana/prometheus \
  --namespace monitoring -f values/prometheus-values.yaml

# Install Loki + MinIO + Gateway
helm upgrade --install loki grafana/loki \
  --namespace monitoring -f values/loki-values.yaml

# Install Promtail
helm upgrade --install promtail grafana/promtail \
  --namespace monitoring -f values/promtail-values.yaml

# Install Tempo
helm upgrade --install tempo grafana/tempo \
  --namespace monitoring -f values/tempo-values.yaml

# Install Pyroscope
helm upgrade --install pyroscope grafana/pyroscope \
  --namespace monitoring -f values/pyroscope-values.yaml

# Install Grafana
helm upgrade --install grafana grafana/grafana \
  --namespace monitoring -f values/grafana-values.yaml
```

## 🔐 Access Grafana

Grafana is exposed as a ClusterIP service. You can access it using port-forward:

```bash
kubectl port-forward -n monitoring svc/grafana 3000:80
```

Then open [http://localhost:3000](http://localhost:3000) and login with:

- **Username:** `admin`
- **Password:** `admin` (or your configured password)

## 📚 Articles

This stack is documented in a 4-part blog series:

1. [**Part 1: Configuring Prometheus & Loki with Grafana**](./docs/01-prometheus-loki-grafana.md)
2. [**Part 2: Configuring Tempo for Distributed Tracing**](./docs/02-tempo.md)
3. [**Part 3: Configuring Pyroscope for Continuous Profiling**](./docs/03-pyroscope.md)
4. [**Part 4: Putting it All Together - Full Observability Stack**](./docs/04-observability-stack.md)

## 📂 Directory Structure

```
.
├── values/
│   ├── grafana-values.yaml
│   ├── loki-values.yaml
│   ├── promtail-values.yaml
│   ├── prometheus-values.yaml
│   ├── tempo-values.yaml
│   └── pyroscope-values.yaml
├── docs/
│   ├── 01-prometheus-loki-grafana.md
│   ├── 02-tempo.md
│   ├── 03-pyroscope.md
│   └── 04-observability-stack.md
└── README.md
```

## 🙌 Contributing

Feel free to open issues or pull requests to suggest improvements or report bugs.

---

Made with ❤️ for DevOps and SREs.

