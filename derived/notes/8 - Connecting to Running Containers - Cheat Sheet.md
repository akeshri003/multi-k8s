# Connecting to Running Containers Cheat Sheet

## Source

- Transcript: `raw/transcripts/8 - Connecting to Running Containers.md`
- Wiki source page: [[8 - Connecting to Running Containers]]
- Key visuals:
  - ![[raw/assets/Screenshot 2026-04-17 at 10.48.55 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 10.53.07 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 10.55.35 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 10.58.19 PM.png]]

## Core Idea

This lesson shows the first full loop:

1. apply the pod and service manifests
2. verify them with `kubectl get`
3. open the app through the service's `nodePort`

## Main Commands

```bash
kubectl apply -f client-pod.yaml
kubectl apply -f client-node-port.yaml
kubectl get pods
kubectl get services
minikube ip
```

## Key Output

```text
pod/client-pod created
service/client-node-port created
```

## Access Model

- `kubectl apply -f`
  Loads the manifest into the cluster.
- `kubectl get pods`
  Confirms pod readiness, running state, restarts, and age.
- `kubectl get services`
  Confirms the service exists and shows the service `port` and `nodePort`.
- `minikube ip`
  Gives the VM IP for browser access when the cluster runs inside `minikube`.

## Port Reminder

- `3050`
  Service port used by other cluster objects.
- `31515`
  External `nodePort` used by the developer in the browser.
- `targetPort`
  Still matters for routing, but is not shown in the service status output.

## Runtime Caveat

- In a `minikube` VM setup, do not assume `localhost`.
- In Docker Desktop Kubernetes, `localhost` may still work.

## Retrieval Cues

- "apply manifests, then verify"
- "`kubectl get` is the status check"
- "`nodePort` is the browser entry point"
- "`minikube` uses VM IP, not localhost"

