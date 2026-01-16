# Day 02: Kubernetes Networking, Scheduling & Storage

## 🚀 Overview
Day 02 focuses on intermediate Kubernetes concepts essential for configuring robust applications. The practice involved debugging connectivity issues, managing pod scheduling via node affinity, and configuring persistent storage for stateful workloads.

## 🛠️ Scenarios & Troubleshooting

### Incident #003: Service Discovery Failure (Networking)
- **Problem:** The `backend-service` was created successfully but could not route traffic to the pods. Endpoints were listed as `<none>`.
- **Root Cause:** Label Mismatch. The Service selector looked for `app: backend-api`, but the Pods were labeled `app: backend-v1`.
- **Resolution:** Updated the Service selector to match the Pod labels exactly.
- **Key Concept:** Services rely on exact label matching to register Pod IP addresses as Endpoints.

### Incident #004: Pod Scheduling Stuck in "Pending" (Scheduling)
- **Problem:** The `heavy-db` pod remained in a `Pending` state and would not start.
- **Diagnosis:** Running `kubectl describe pod heavy-db` revealed specifically: `0/1 nodes are available: node(s) didn't match node selector`.
- **Root Cause:** The pod specification included a `nodeSelector` for `disktype: ssd`, but no nodes in the cluster had this label.
- **Resolution:**
    1. Identified the node name: `kubectl get nodes`.
    2. Applied the missing label: `kubectl label nodes <node-name> disktype=ssd`.
- **Key Concept:** The Scheduler filters out nodes that do not satisfy `nodeSelector` or `affinity` rules.

### Incident #005: Stateful Data Persistence (Storage)
- **Problem:** Deploying a Database (MySQL) requires data to survive Pod restarts. Ephemeral storage is insufficient.
- **Solution:** Implemented a **PersistentVolumeClaim (PVC)**.
    - Created a PVC `mysql-pvc` requesting 1Gi of storage.
    - Configured the MySQL pod to mount this claim at `/var/lib/mysql`.
- **Outcome:** The PVC successfully bound to a standard StorageClass (`local-path`), ensuring data persistence.
- **Verified via:** `kubectl describe pvc mysql-pvc` showing Status: `Bound`.

## 📂 Configuration Files
| File | Description |
|------|-------------|
| `Networking.yml` | Demonstrates Service-to-Pod label matching and ClusterIP configuration. |
| `Pending-task.yml` | Demonstrates Node Selection constraints and scheduling logic. |
| `Storage-task.yml` | Demonstrates PersistentVolumeClaims (PVC) and Volume Mounting for databases. |

## 💻 Commands Mastered
```bash
# Debugging Endpoints
kubectl get endpoints <service-name>

# Node Management
kubectl get nodes --show-labels
kubectl label nodes <node-name> <key>=<val>

# Storage Verification
kubectl get pvc
kubectl describe pvc <pvc-name>
```