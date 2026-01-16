# Day 02: Kubernetes Pod Networking

## Overview
This day focuses on understanding how pods communicate within a Kubernetes cluster using Services and DNS.

## Exercise: Service Discovery and Connectivity

### Components
1. **Backend Pod**: Runs Nginx, exposed via a Service.
2. **Backend Service**: ClusterIP service named `backend-svc`.
3. **Client Pod**: Busybox pod used to test connectivity.

### Tasks
1. **Apply the configuration**:
   ```bash
   kubectl apply -f Networking.yml
   ```

2. **Verify Pods and Services**:
   Ensure both pods are running and the service has a ClusterIP.
   ```bash
   kubectl get pods -o wide
   kubectl get svc
   ```

3. **Test Connectivity via IP**:
   Get the IP of the `backend-pod` and try to access it from `client-pod`.
   ```bash
   # Get IP
   kubectl get pod backend-pod -o jsonpath='{.status.podIP}'

   # Exec into client
   kubectl exec -it client-pod -- sh

   # Inside client-pod (use the IP retrieved above)
   wget -qO- <backend-pod-ip>
   ```

4. **Test Connectivity via Service Name (DNS)**:
   This demonstrates Kubernetes internal DNS resolution.
   ```bash
   # Inside client-pod
   wget -qO- http://backend-svc
   nslookup backend-svc
   ```

## Key Concept
Pods in a cluster can communicate with each other directly via IP (unreliable as IPs change) or consistently via **Services** (DNS names).

## Commands Used
```bash
kubectl apply -f Networking.yml
kubectl exec -it client-pod -- sh
```
