# ClusterIP for Express API Cheat Sheet

## Source

- Transcript: `raw/transcripts/31 - ClusterIP for Express API.md`
- Wiki source page: [[31 - ClusterIP for Express API]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 8.46.02 PM.png]]

## Core Idea

This lesson creates the internal service for the Express API.

It is the backend version of the earlier frontend `ClusterIP` service.

## Main YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: server-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: server
  ports:
    - port: 5000
      targetPort: 5000
```

## What Each Part Means

- `name: server-cluster-ip-service`
  the backend service object
- `type: ClusterIP`
  internal-only access
- `selector: component: server`
  send traffic to the backend pods
- `port: 5000`
  other objects in the cluster talk to the service on `5000`
- `targetPort: 5000`
  the service forwards traffic to port `5000` on the backend pods

## Why Port `5000`

- the `multi-server` image listens on `5000`
- so both service port and target port are kept at `5000`

## Why This Is Internal

- the Express API should not be directly exposed with `NodePort`
- it should stay behind a `ClusterIP`
- later, `Ingress` will handle outside-facing routing

## Retrieval Cues

- "backend service mirrors frontend ClusterIP service"
- "`component: server`"
- "`server-cluster-ip-service`"
- "port `5000` in, port `5000` out"
