# Applying a PVC Cheat Sheet

## Source

- Transcript: `raw/transcripts/45 - Applying a PVC.md`
- Wiki source page: [[45 - Applying a PVC]]

## Core Idea

This lesson is the first runtime check for the PVC setup.

It proves that:

- the claim exists
- the actual persistent volume exists
- the volume is bound to the claim

## Main Command Flow

```bash
kubectl apply -f k8s
kubectl get pods
kubectl get pv
kubectl get pvc
```

## What Each Command Checks

- `kubectl apply -f k8s`
  - apply the updated config
- `kubectl get pods`
  - make sure Postgres restarted and is running
- `kubectl get pv`
  - see the actual persistent volume instance
- `kubectl get pvc`
  - see the claim object

## What Matters Most

- the `pv` should show the claim on the right
- the `pv` status should be `Bound`
- the `pvc` is the request
- the `pv` is the real storage

## Retrieval Cues

- "apply then inspect"
- "`pv` = actual storage"
- "`pvc` = request"
- "`Bound` means the claim is in use"
