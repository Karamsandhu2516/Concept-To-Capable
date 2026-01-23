#### What is a Pod, and why don't we just run containers directly?
A: A Pod is the smallest deployable unit in K8s. It provides a shared network and storage environment for one or more containers (like an app and a sidecar) to work as a single logical host.

#### Explain the difference between a Deployment and a Pod.
A: A Pod is a single instance. A Deployment is a controller that manages Pods, ensuring the desired number of replicas are running and handling rolling updates.

#### What is the purpose of a Service in Kubernetes?
A: Services provide a stable IP address and DNS name for a set of Pods, which are otherwise ephemeral and change IPs whenever they restart.

#### What are the different types of Services?
A: ClusterIP (Internal only), NodePort (Exposed on a port on each Node), and LoadBalancer (Exposed via a Cloud Provider's LB).

#### When would you use a NodePort vs. a ClusterIP?
A: Use ClusterIP for internal communication between microservices. Use NodePort (or LoadBalancer) when you need to expose the service to external traffic outside the cluster.

#### What is a ConfigMap, and how is it different from a Secret?
A: ConfigMaps store non-sensitive configuration data (like nginx.conf). Secrets store sensitive data (like passwords or API keys) and are base64 encoded.

#### What does "Ephemeral" mean regarding Kubernetes Pods?
A: It means Pods are temporary. If a Pod is deleted or crashes, any data stored in its local container filesystem is lost forever.

#### Explain the relationship between a PersistentVolume (PV) and a PersistentVolumeClaim (PVC).
A: A PV is the actual storage resource (the "Hard Drive"). A PVC is a request for storage by a user (the "Ticket"). Kubernetes matches the PVC to a suitable PV.

#### What is the "Sidecar Pattern"?
A: It is an architecture where an extra container is added to a Pod to enhance the main application (e.g., a logging agent or a Prometheus exporter) without modifying the main app's code.

#### How do containers in the same Pod communicate?
A: They share the same network namespace and communicate via localhost.

#### If a PVC is stuck in Pending status, what are your troubleshooting steps?
A: I check kubectl describe pvc to look for events. Common causes include no available PVs matching the size/access mode or a missing StorageClass.

#### A developer says their Grafana dashboard is empty. How do you investigate?
A: 1. Verify the Pod is Running (2/2 READY). 2. Check the Service Endpoints (kubectl get ep) to ensure the metrics port is listed. 3. Check the Prometheus UI /targets page to see if the ServiceMonitor is successfully scraping the pod.

#### What is ReadWriteOnce (RWO), and what happens if you have replicas on different nodes?
A: RWO means the volume can be mounted by only one node at a time. If replicas are scheduled on different nodes, the pods on the second node will fail to start because they cannot "attach" to the locked volume.

#### How do you debug a Pod that is in CrashLoopBackOff?
A: Use kubectl logs <pod-name> --previous to see the logs from the last crashed instance, and kubectl describe pod to check for resource limits (OOMKilled) or configuration errors.
