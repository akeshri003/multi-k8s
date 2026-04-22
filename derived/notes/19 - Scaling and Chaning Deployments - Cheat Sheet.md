# Scaling and Chaning Deployments Cheat Sheet

## Source

- Transcript: `raw/transcripts/19 - Scaling and Chaning Deployments.md`
- Wiki source page: [[19 - Scaling and Chaning Deployments]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 3.41.19 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 3.41.34 PM.png]]

## Core Idea

This lesson shows three important deployment behaviors:

1. changing the pod template recreates pods
2. changing `replicas` scales the number of pods
3. changing the image triggers a rollout where old and new pods briefly coexist

## Main Commands

```bash
kubectl apply -f client-deployment.yaml
kubectl get deployments
kubectl get pods
kubectl describe pods
```

## What Happens After The Port Change

- the deployment file is edited
- `kubectl apply -f client-deployment.yaml` says the deployment was `configured`
- `kubectl get pods` shows a very new pod age, such as `26s`
- that means the old pod was replaced by a new pod from the updated template
- `kubectl describe pods` is then used to check the new pod details

## What Happens After Scaling

- `replicas` changes from `1` to `5`
- re-applying the manifest makes the deployment create five pods
- `kubectl get pods` now shows five separate pod instances from the same template

## What Happens During The Image Rollout

The image changes from `multi-client` to `multi-worker`.

The interesting deployment snapshot is:

- `desired = 5`
- `current = 7`
- `up-to-date = 4`
- `available = 4`

Simple reading:

- the deployment wants 5 pods total
- 4 pods already match the newest template
- 3 older pods are still around for a moment
- after cleanup finishes, the status should settle back to 5 across the board

## Important Caveats

- the transcript says `99999` once but later reasons about `999`
- the exact replacement port value is inconsistent in the raw source
- the bigger lesson is not the port number itself
- the bigger lesson is that deployments replace pods when the template changes

## Retrieval Cues

- "deployment template change recreates pods"
- "`replicas` scales pod count"
- "`current` can be larger than `desired` during rollout"
- "old and new pods can briefly exist at the same time"
