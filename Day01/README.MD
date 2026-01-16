# Day 01: Kubernetes Pod Basics

## Overview
This day focuses on fundamental Kubernetes pod concepts, including pod debugging and resource management.

## Exercise 1: Pod Debugging

### Scenario: Frontend Webapp ImagePullBackOff

#### Components Deployed:
- **Pod**: `frontend-webapp`
  - **Image**: nginx:1.23.4-non-existent-tag (initially)
  - **Port**: 80

#### The Issue: Image Pull Failure
- **Symptom**: Pod stuck in `ErrImagePull` state
- **Root Cause**: Invalid image tag `nginx:1.23.4-non-existent-tag`
- **Impact**: Pod cannot start because the specified image doesn't exist

#### Debugging Steps:
1. Check pod status:
   ```bash
   kubectl get pods
   ```

2. Describe the pod for more details:
   ```bash
   kubectl describe pod frontend-webapp
   ```

3. Check pod logs:
   ```bash
   kubectl logs frontend-webapp
   ```

#### The Fix:
Update the image tag in the pod specification to a valid tag (e.g., `nginx:latest`)

## Exercise 2: Resource Management (OOM)

### Scenario: Memory Limit Exceeded

#### Components Deployed:
- **Pod**: `memory-hog-api`
  - **Image**: polinux/stress
  - **Memory Limit**: 100Mi
  - **Memory Request**: 10Mi

#### The Issue: Container OOMKilled
- **Symptom**: Pod status shows `OOMKilled`
- **Root Cause**: Container exceeded its memory limit
- **Impact**: Container is terminated due to out-of-memory condition

#### Debugging Steps:
1. Check pod status:
   ```bash
   kubectl get pods
   ```

2. Describe the pod for more details:
   ```bash
   kubectl describe pod memory-hog-api
   ```

3. Check container logs:
   ```bash
   kubectl logs memory-hog-api
   ```

#### The Fix:
1. Increase memory limits in the pod specification
2. Optimize application memory usage
3. Consider implementing resource requests and limits appropriately

## Key Learnings:
- Always use valid image tags
- Understand the importance of resource requests and limits
- Monitor pod status and logs for debugging
- Use `kubectl describe` for detailed pod information

## Commands Used:
```bash
# Apply configurations
kubectl apply -f Day01-Pod-Debug.yml
kubectl apply -f Day01-OOM-Task.yml

# Check pod status
kubectl get pods

# Get detailed pod information
kubectl describe pod <pod-name>

# View pod logs
kubectl logs <pod-name>
```
