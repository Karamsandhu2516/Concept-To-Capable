## Day 02: Networking & Scheduling

### Incident #003: Service Endpoints Missing (Ghost Service)
- **Symptom:** Service existed but had no endpoints (`<none>`).
- **Diagnosis:** Service selector `app: backend-api` did not match Pod label `app: backend-v1`.
- **Fix:** Aligned labels and selectors.

### Incident #004: Pod Stuck in Pending
- **Symptom:** Status `Pending`, no container started.
- **Diagnosis:** `kubectl describe` showed `0/1 nodes are available: 1 node(s) didn't match Pod's node affinity/selector`.
- **Fix:** Removed the invalid `nodeSelector`.