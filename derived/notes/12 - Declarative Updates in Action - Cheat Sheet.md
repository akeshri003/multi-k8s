# Declarative Updates in Action Cheat Sheet

## Source

- Transcript: `raw/transcripts/12 - Declarative Updates in Action.md`
- Wiki source page: [[12 - Declarative Updates in Action]]
- Key visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.04.42 PM.png]]
  - ![[Screenshot 2026-04-19 at 3.05.02 PM.png]]

## Core Idea

Declarative update workflow:

1. edit the original config file
2. apply it again
3. verify that the old object was updated

## Example Change

The lesson changes the pod image from:

```text
stephengrider/multi-client
```

to:

```text
stephengrider/multi-worker
```

## Main Commands

```bash
kubectl get pods
kubectl get services
docker ps
kubectl apply -f client-pod.yaml
kubectl get pods
kubectl describe pod client-pod
```

## Most Important Output

```text
pod/client-pod configured
```

That means Kubernetes updated the existing configuration for `client-pod`.

## How The Lesson Verifies The Update

- still only one pod named `client-pod`
- restart count goes up
- `kubectl describe pod client-pod` shows:

```text
Image: stephengrider/multi-worker
```

and events like:

```text
Container client definition changed, will be restarted
Pulling image "stephengrider/multi-worker"
```

## Best Mental Model

- `kubectl apply`
  sends the changed desired state
- `kubectl get`
  confirms the object still exists
- `kubectl describe`
  proves what changed inside that object

## Retrieval Cues

- "configured means updated"
- "same pod name, new image"
- "describe is the detailed inspection command"
- "events show the restart and image pull"

