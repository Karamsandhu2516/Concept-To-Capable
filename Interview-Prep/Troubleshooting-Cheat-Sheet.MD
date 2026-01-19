# 🛠️ Kubernetes Troubleshooting Cheat Sheet

This guide serves as my primary reference for diagnosing and resolving issues within the cluster, categorized by the "layer" of the failure.

---

## 🩺 Level 1: Pod & Container Health
*Use these when a pod is stuck in `Pending`, `CrashLoopBackOff`, or `ImagePullBackOff`.*

| Command | Purpose |
| :--- | :--- |
| `kubectl get pods -A` | Lists all pods in all namespaces to check for cluster-wide system failures. |
| `kubectl describe pod <pod-name>` | **The First Step.** Provides the "Events" log showing scheduling errors, volume mounting failures, or health check timeouts. |
| `kubectl logs <pod-name> --previous` | Retrieves logs from a crashed container instance. Essential for debugging `Exit Code 1` or `137`. |
| `kubectl get events --sort-by='.lastTimestamp'` | Shows a chronological timeline of all errors and warnings occurring in the namespace. |

---

## 🌐 Level 2: Networking & Service Discovery
*Use these when pods are running but cannot communicate, or when external traffic isn't reaching the app.*

| Command | Purpose |
| :--- | :--- |
| `kubectl get endpoints <svc-name>` | Verifies if a Service has successfully "found" its Pods. If `<none>`, check Label Selectors. |
| `kubectl get svc -o wide` | Displays internal ClusterIPs, External IPs, and Port mappings (Port vs TargetPort vs NodePort). |
| `kubectl run -it --rm debug --image=busybox -- sh` | Launches a temporary "Swiss Army Knife" pod inside the cluster to test `ping`, `nslookup`, or `wget` connectivity. |
| `kubectl describe svc <svc-name>` | Checks the specific selector and port configuration of a Service. |

---

## 💾 Level 3: Storage & Persistence
*Use these when pods are stuck in `ContainerCreating` due to volume issues.*

| Command | Purpose |
| :--- | :--- |
| `kubectl get pvc,pv` | Confirms if a PersistentVolumeClaim is `Bound`. A `Pending` status usually indicates a StorageClass or capacity issue. |
| `kubectl describe pvc <pvc-name>` | Reveals why a disk failed to attach (e.g., Zone mismatch or Access Mode conflicts). |

---

## 📈 Level 4: Observability & Management
*Use these to manage the monitoring stack and verify cluster-wide alerting logic.*

| Command | Purpose |
| :--- | :--- |
| `helm list -A` | Displays all deployed Helm charts and their current status/revision. |
| `helm history <release-name>` | Provides a version history for rollbacks if a configuration change breaks the stack. |
| `kubectl get prometheusrules` | Lists the logic triggers that Alertmanager uses to notify engineers of failures. |

---

## 💡 Top 3 "In-the-Trenches" Commands
1. **`kubectl describe...`**: To find out *what* happened (Events).
2. **`kubectl get endpoints...`**: To check if the networking "bridge" is broken.
3. **`kubectl logs --previous`**: To find out *why* a container died (Post-mortem).

---
