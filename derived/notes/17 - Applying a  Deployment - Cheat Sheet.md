# Applying a Deployment Cheat Sheet

## Source

- Transcript: `raw/transcripts/17 - Applying a  Deployment.md`
- Wiki source page: [[17 - Applying a Deployment]]
- Key visual:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.44.56 PM.png]]

## Core Idea

This lesson does three practical things:

1. deletes the old direct pod
2. applies the deployment manifest
3. reads deployment status with `kubectl get deployments`

## Main Commands

```bash
kubectl get pods
kubectl delete -f client-pod.yaml
kubectl get pods
kubectl apply -f client-deployment.yaml
kubectl get pods
kubectl get deployments
```

## Important Output

```text
pod "client-pod" deleted from default namespace
```

and then a new deployment-managed pod appears with a generated name.

## Why The Delete Step Exists

- remove the old manually created pod
- keep the cluster state easier to read
- avoid confusing the old pod with the new deployment-managed pod

## What `kubectl delete -f` Means Here

- Kubernetes reads the manifest
- it uses the object's `kind` and `name`
- it finds the matching object
- it deletes it

The lesson treats this as an imperative action.

## `kubectl get deployments`

New status columns:

- `desired`
  target pod count from `replicas`
- `current`
  number of pods currently running
- `up-to-date`
  number of pods using the latest deployment template
- `available`
  number of pods ready to accept traffic

## Retrieval Cues

- "delete old pod before applying deployment"
- "deployment creates pod with generated name"
- "`kubectl get deployments` shows deployment health"
- "`desired/current/up-to-date/available` are the key columns"

