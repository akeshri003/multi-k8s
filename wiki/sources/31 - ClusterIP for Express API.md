---
title: ClusterIP for Express API
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/31 - ClusterIP for Express API.md
source_date: 2026-04-19
tags:
  - kubernetes
  - service
  - clusterip
  - express
  - api
  - backend
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/28 - The ClusterIP Config.md
  - wiki/sources/30 - Express API deployment config.md
---

# ClusterIP for Express API

## Metadata

- Source type: transcript with embedded YAML and supporting service diagram
- Transcript quality: strong enough to preserve the full backend `ClusterIP` service manifest and its relation to the `server-deployment`
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 8.46.02 PM.png]]
- Derived notes:
  - `derived/notes/31 - ClusterIP for Express API - Cheat Sheet.md`
  - `derived/notes/31 - ClusterIP for Express API - Explainer.md`

## Summary

This source creates the internal `ClusterIP` service for the Express API. It mirrors the frontend `ClusterIP` pattern almost exactly, but now targets the backend pods labeled `component: server` and routes traffic through port `5000`, which is the port the `multi-server` image listens on. The lesson’s main point is that the backend API should stay internal to the cluster, so it gets a `ClusterIP` service rather than a `NodePort`.

## Key Claims

- The Express API should be fronted by a `ClusterIP` service, not a `NodePort`.
- The backend service should be named `server-cluster-ip-service`.
- The service selector should use `component: server` so it targets the backend pods created by `server-deployment`.
- The `multi-server` image listens on port `5000`.
- The service should expose port `5000` and forward traffic to `targetPort: 5000`.
- The backend `ClusterIP` service is almost identical to the earlier frontend `ClusterIP` service, except for the name, selector value, and port number.

## Important Evidence Or Examples

- The source includes this service manifest:

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

- The lesson explicitly ties the selector back to the previous deployment:

```yaml
selector:
  component: server
```

- The service is described as the object that will create and maintain internal access to the running Express API pods.
- The supporting diagram shows the internal routing path:
  - another object in the cluster talks to port `5000`
  - the `ClusterIP` service receives that traffic
  - traffic is forwarded to the `multi-server` pods on port `5000`
- The transcript repeatedly emphasizes that this config is intentionally almost identical to the earlier frontend `ClusterIP` service in order to reinforce the pattern.

## Important Commands

- No shell command is central in this source.
- The lesson is focused on writing the backend service manifest.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson finishes the backend internal service, but it still does not show how outside requests will ultimately reach the backend through `Ingress`.
- The source shows the backend service name and selector, but it still does not explain how other pods will discover and call this service by name.
- The backend deployment and backend service now both exist structurally, but the API still needs Redis/Postgres environment variables before the overall app wiring is complete.
