---
title: Express API deployment config
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/30 - Express API deployment config.md
source_date: 2026-04-19
tags:
  - kubernetes
  - deployment
  - express
  - api
  - backend
  - clusterip
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/24 - The Path to Production.md
  - wiki/sources/28 - The ClusterIP Config.md
  - wiki/sources/29 - Applying Multiple Files with Kubectl.md
---

# Express API deployment config

## Metadata

- Source type: transcript with embedded screenshots and deployment YAML
- Transcript quality: strong enough to preserve the architecture placement of `multi-server`, the full backend deployment manifest, and the note that environment variables will be added later
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 8.45.18 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 8.46.02 PM.png]]
- Derived notes:
  - `derived/notes/30 - Express API deployment config - Cheat Sheet.md`
  - `derived/notes/30 - Express API deployment config - Explainer.md`

## Summary

This source creates the deployment config for the Express API, using the `multi-server` image as the next major workload in the larger Fibonacci app architecture. The manifest closely follows the same pattern as the earlier frontend deployment: `apps/v1`, `kind: Deployment`, `replicas: 3`, `selector.matchLabels`, and a pod template with one container. The key differences are the label value `component: server`, the container name `server`, and the hard-coded container port `5000`. The lesson also explicitly says the deployment is still incomplete because Redis and Postgres connection environment variables will be added later.

## Key Claims

- The Express API should be deployed with a `Deployment`, not exposed directly through a `NodePort`.
- The corresponding service for the Express API should be a `ClusterIP` service because the API is not meant to be directly accessible from outside the cluster.
- The `multi-server` image listens on port `5000`.
- The backend deployment should initially run three pod replicas.
- The deployment selector should use `component: server`.
- The pod template labels should also use `component: server` so the deployment can manage the correct pods.
- The backend pod only needs one container, the `multi-server` image.
- The deployment config is still incomplete until environment variables for Redis and Postgres are added.

## Important Evidence Or Examples

- The source includes this deployment manifest:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: server-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      component: server
  template:
    metadata:
      labels:
        component: server
    spec:
      containers:
        - name: server
          image: stephengrider/multi-server
          ports:
            - containerPort: 5000
```

- One embedded architecture diagram places `multi-server` behind its own `ClusterIP` service, parallel to the `multi-client` deployment.
- The second diagram shows the internal networking path:
  - another object inside the cluster talks to port `5000`
  - the `ClusterIP` service receives that traffic
  - traffic is forwarded to port `5000` on the `multi-server` pods
- The transcript explains that `component: server` is just one label choice; the label key/value could technically be anything, but this naming is chosen for clarity.
- The transcript explicitly says the deployment file is not finished yet because Redis and Postgres connection details still need to be injected as environment variables.
- The transcript appears to mis-hear the intended filename as `server-deployment.yamo`; the surrounding explanation makes `server-deployment.yaml` the intended filename.

## Important Commands

- No shell command is central in this source.
- The lesson is focused on writing the backend deployment manifest, not applying it yet.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The lesson creates the backend deployment, but it does not yet show the matching `ClusterIP` service manifest for the Express API.
- The source deliberately postpones the environment-variable section, so the deployment is structurally correct but not yet fully runnable in the final architecture.
- The lesson explains that the API is internal-only, but it still does not show the exact path by which `Ingress` and the frontend will talk to this backend service once the routing layer is added.
