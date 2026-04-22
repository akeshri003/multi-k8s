---
title: The Worker Deployment
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/33 - The Worker Deployment.md
source_date: 2026-04-19
tags:
  - kubernetes
  - deployment
  - worker
  - backend
  - scaling
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/30 - Express API deployment config.md
  - wiki/sources/31 - ClusterIP for Express API.md
---

# The Worker Deployment

## Metadata

- Source type: transcript with embedded deployment YAML
- Transcript quality: strong enough to preserve the worker deployment structure, the scaling rationale, and the important claim that the worker needs no service
- Embedded visuals:
  - None explicitly provided in the transcript or newly added assets
- Derived notes:
  - `derived/notes/33 - The Worker Deployment - Cheat Sheet.md`
  - `derived/notes/33 - The Worker Deployment - Explainer.md`

## Summary

This source creates the deployment for the `multi-worker` part of the app. It follows the same deployment pattern as the client and server manifests, but starts with only one replica, uses the label `component: worker`, and deliberately omits both a service and any exposed ports. The lesson’s main architectural point is that the worker only makes outgoing connections and does not need other objects to initiate requests into it, so there is no reason to put a service in front of it.

## Key Claims

- The worker should be deployed with a `Deployment`.
- The initial replica count for the worker is `1`.
- The worker is the most likely part of the app to need scaling later because it performs the expensive Fibonacci calculation.
- The deployment selector should use `component: worker`.
- The pod template labels should also use `component: worker`.
- The deployment runs one container named `worker` from the `multi-worker` image.
- The worker still needs Redis-related environment variables later, but those are intentionally deferred.
- The worker does not need a service.
- The worker does not need any port configuration because nothing else in the cluster makes direct inbound requests to it.

## Important Evidence Or Examples

- The source includes this worker deployment manifest:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: worker-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: worker
  template:
    metadata:
      labels:
        component: worker
    spec:
      containers:
        - name: worker
          image: stephengrider/multi-worker
```

- The transcript explains the scaling rationale explicitly:
  - the worker performs the slow Fibonacci calculation
  - that makes it the most likely part of the app to scale later
  - but the course starts at one replica so that future scaling remains visible and meaningful
- The source makes an explicit service distinction:
  - other objects do not initiate requests into the worker
  - the worker initiates requests outward
  - therefore no service is needed
- The transcript also says no port mapping is needed for the worker because there is no inbound traffic path to expose.
- The raw pasted YAML in the transcript drops the indentation of `template`, but the spoken explanation is clear that `template` belongs at the same level as `selector`, so the normalized version above reflects the intended structure.

## Important Commands

- No shell command is central in this source.
- The lesson is about writing the worker deployment manifest.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The lesson makes a strong case that the worker needs no service, but it still does not show the exact Redis environment variables the worker will need in order to do useful work.
- The source says the worker is the most likely thing to scale later, but it still does not explain what scaling signals or queue behavior will justify that decision.
- The worker deployment is structurally complete, but the broader app still lacks the Redis and Postgres wiring required for the backend pieces to function together.
