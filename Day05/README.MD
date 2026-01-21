# Day 05: Sidecar Observability & Grafana Dashboards

## 🎯 Project Goal
The goal of this lab was to implement the **Sidecar Design Pattern** to enable monitoring for an Nginx application using **Prometheus Operator** and **Grafana**. 

## 🛠️ Issues Encountered & Technical Fixes

### 1. The Metrics Discovery Problem
**Problem:** Nginx doesn't speak "Prometheus". It provides a status page, but Prometheus cannot read it directly.
**YAML Fix (Sidecar):**
I added a second container (`nginx-exporter`) to the same Pod. Because containers in a Pod share the same `localhost`, the sidecar can read Nginx's status and translate it into a format Prometheus understands on port `9113`.

### 2. The Scraping Connection
**Problem:** Prometheus wasn't "looking" at pods.
**YAML Fix (ServiceMonitor):**
created a `ServiceMonitor` object. The critical detail here was the label:
```yaml
labels:
  release: my-monitor # This MUST match the name of Helm release
```
This label acts as a "subscription" that tells the Prometheus Operator to automatically add  Nginx pods to its scrape list.

### 3. The "No Data" Mystery in Grafana
**The Issue:** The standard Grafana dashboards (like *Kubernetes / Compute Resources / Pod*) were showing "No Data" even though Prometheus was healthy.
**The Technical Reason:** 
*   Standard dashboards rely on **Recording Rules** (pre-calculated metrics).
*   In local environments (like Orbstack or Docker Desktop), these rules often fail because of missing labels (like `container_name` vs `container`) or resource constraints on the laptop.
*   The dashboard was looking for a pre-calculated aggregate that didn't exist in local cluster.

### 4. The Custom Dashboard Fix
**The Fix:** created a dedicated **"NGINX Monitoring"** dashboard.
**Why this solved it:**
Instead of using the broken "shortcuts" (Recording Rules), wrote raw PromQL queries that look directly at the sidecar data:
`sum(rate(nginx_http_requests_total{job="web-entry"}[1m])) by (pod)`
By bypassing the enterprise-scale shortcuts, we got immediate, accurate visibility into our local application traffic.

## 📊 Visualization Highlights
*   **Real-time Traffic:** Total requests per second across all workers.
*   **Pod Distribution:** Ability to see which specific pod is handling the most traffic.
*   **Health Checks:** Confirmation that the sidecar is successfully talking to the main Nginx container.

## 🚀 How to Access
1. **Apply Configs:** `kubectl apply -f Day05/deploy.yaml`
2. **Port Forward:** `kubectl port-forward svc/my-monitor-grafana 3001:80`
3. **Login:** [http://localhost:3001](http://localhost:3001) (Admin/prom-operator)
4. **Dashboard:** Select the **"NGINX Monitoring"** dashboard from the search menu.
