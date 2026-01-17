# 📊 Day 03: Kubernetes Observability with Prometheus & Grafana

## 🎯 Project Overview
Today’s focus was moving from manual troubleshooting to **automated observability**. In a production environment, you cannot wait for a pod to crash to know something is wrong. I deployed an industry-standard monitoring stack to gain real-time visibility into the "vital signs" of my cluster.

---

## 🛠️ The Tech Stack
* **Helm v4.0.4**: Utilized as the Kubernetes package manager to automate the deployment of complex monitoring infrastructure.
* **Prometheus**: Functioning as the time-series database to "scrape" and store metrics from the cluster.
* **Grafana**: Used to visualize data through professional-grade dashboards.
* **Kube-State-Metrics & Node-Exporter**: Tools that translate hardware and Kubernetes object statuses into data Prometheus can read.

---

## 🚀 Implementation Steps

### 1. Infrastructure Deployment
I added the official Prometheus community charts and used Helm to install the full stack:
```bash
helm repo add prometheus-community [https://prometheus-community.github.io/helm-charts](https://prometheus-community.github.io/helm-charts)
helm repo update
helm install my-monitor prometheus-community/kube-prometheus-stack