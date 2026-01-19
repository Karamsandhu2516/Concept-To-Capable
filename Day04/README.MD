# Day 04: Services, Labels, and Selectors

## 🎯 Objective
The goal of Day 04 is to understand how **Kubernetes Services** discover and connect to **Pods**. 

The core concept learned today is the relationship between **Labels** (on Pods) and **Selectors** (on Services). If these don't match exactly, the Service won't send traffic to the Pod.

## 🏗️ Architecture
We created two resources in a single YAML file (`day04-test.yaml`):

1.  **Pod (`web-worker`)**: An Nginx web server.
2.  **Service (`web-entry`)**: A NodePort service that exposes the Pod to the outside world.

### Key Concepts

#### 1. Labels: The "ID Card" 🏷️
Labels are key-value pairs attached to objects.
In our Pod definition:
```yaml
metadata:
  labels:
    env: production 
    tier: frontend
```
The `env: production` label acts as the identifier.

#### 2. Selectors: The "Search Query" 🔍
The Service uses a **selector** to find which Pods to route traffic to. 
In our Service definition:
```yaml
spec:
  selector:
    env: production
```
**CRITICAL LEARNING:** The Service's `selector` MUST match the Pod's `label` exactly. We fixed a bug where the selector was `env: prod` (which found no pods) and changed it to `env: production`.

#### 3. Ports 🔌
- **`port` (80)**: The port the Service exposes inside the cluster.
- **`targetPort` (80)**: The port the container is actually listening on (must match `containerPort`).
- **`nodePort` (30007)**: The static port opened on every node (your laptop).

## 🚀 How to Run

1.  **Apply the configuration:**
    ```bash
    kubectl apply -f day04-test.yaml
    ```

2.  **Verify the Pod and Service:**
    ```bash
    kubectl get pods
    kubectl get svc
    ```

3.  **Check the Endpoints:**
    This confirms the Service found the Pod correctly.
    ```bash
    kubectl get endpoints web-entry
    ```
    *You should see an IP address listed under ENDPOINTS. If you see `<none>`, the label/selector match is wrong.*

4.  **Access the Application:**
    Open your browser to:
    [http://localhost:30007](http://localhost:30007)

## 🧹 Cleanup
To delete the resources:
```bash
kubectl delete -f day04-test.yaml
```
