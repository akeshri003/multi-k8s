# The ClusterIP Config Cheat Sheet

## Source

- Transcript: `raw/transcripts/28 - The ClusterIP Config.md`
- Wiki source page: [[28 - The ClusterIP Config]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 8.21.07 PM.png]]
- ![[raw/assets/Screenshot 2026-04-17 at 10.12.39 PM.png]]

## Core Idea

This lesson creates the internal service for the frontend.

It takes the earlier idea of `ClusterIP` and turns it into a real config file.

## Main YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: client-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: web
  ports:
    - port: 3000
      targetPort: 3000
```

## What Each Part Means

- `kind: Service`
  this object sets up networking
- `type: ClusterIP`
  this service is only for inside-cluster access
- `selector: component: web`
  send traffic to the frontend pods with that label
- `port: 3000`
  other objects inside the cluster talk to the service on this port
- `targetPort: 3000`
  the service forwards traffic to port `3000` on the target pod

## What Changed From `NodePort`

- `NodePort` had three ports to think about:
  - `port`
  - `targetPort`
  - `nodePort`
- `ClusterIP` drops `nodePort`
- that is because outside users are not supposed to reach this service directly

## Practical Meaning

- browser traffic will not hit this service directly from outside the cluster
- `Ingress` will eventually send traffic to this service
- other objects inside the cluster can also use this service name and port

## Retrieval Cues

- "ClusterIP is internal-only"
- "same selector, different exposure model"
- "`port` is the service port"
- "`targetPort` is the pod port"
- "no `nodePort` because this is not public"
