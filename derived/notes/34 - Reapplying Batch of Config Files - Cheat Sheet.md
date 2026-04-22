# Reapplying Batch of Config Files Cheat Sheet

## Source

- Transcript: `raw/transcripts/34 - Reapplying Batch of Config Files.md`
- Wiki source page: [[34 - Reapplying Batch of Config Files]]

## Core Idea

After adding the new server and worker manifests, the course reapplies the whole `k8s/` folder and checks the results.

## Main Command

```bash
kubectl apply -f k8s
```

## What Happened

```text
service/client-cluster-ip-service unchanged
deployment.apps/client-deployment unchanged
service/server-cluster-ip-service created
deployment.apps/server-deployment created
deployment.apps/worker-deployment created
```

Meaning:

- old frontend objects stay unchanged
- new backend/server objects are created

## Verification Commands

```bash
kubectl get pods
kubectl get deployments
kubectl get services
kubectl logs <pod-name>
```

## What The Cluster Shows

- `client-deployment` is still running with `3/3`
- `server-deployment` is running with `3/3`
- `worker-deployment` is running with `1/1`
- `server-cluster-ip-service` exists on port `5000`

## Important Log Output

```text
Listening
Error: connect ECONNREFUSED 127.0.0.1:5432
```

## Real Meaning

- Kubernetes can say the pod is running
- but the application can still be misconfigured
- the backend still lacks Redis/Postgres wiring

## Retrieval Cues

- "reapply the whole folder again"
- "unchanged objects stay unchanged"
- "running is not the same as fully working"
- "`kubectl logs` is the quick reality check"
