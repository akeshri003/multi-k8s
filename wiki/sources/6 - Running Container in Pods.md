---
title: Running Container in Pods
type: source
status: active
created: 2026-04-17
updated: 2026-04-17
source_path: raw/transcripts/6 - Running Container in Pods.md
source_date: 2026-04-17
tags:
  - kubernetes
  - pod
  - containers
related:
  - wiki/topics/kubernetes.md
---

# Running Container in Pods

## Metadata

- Source type: transcript with supporting screenshots
- Transcript quality: strong enough to preserve the pod mental model and the multi-container pod example
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-17 at 7.48.36 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 8.43.26 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 8.45.37 PM.png]]
- Derived notes:
  - `derived/notes/6 - Running Container in Pods - Cheat Sheet.md`
  - `derived/notes/6 - Running Container in Pods - Explainer.md`

## Summary

This source explains what a pod is and why Kubernetes deploys pods rather than naked standalone containers. Its main point is that pods are the smallest deployable unit in Kubernetes and are intended to group one or more tightly coupled containers.

## Key Claims

- The smallest thing Kubernetes deploys is a `Pod`, not an individual naked container.
- A pod can run one or more containers.
- Multiple containers should share a pod only when they have a very tight relationship and must run together.
- The earlier multi-container app is not a good example of "put everything in one pod" because many parts could still function in degraded form if another part failed.
- A stronger example of a multi-container pod is a primary database plus tightly coupled support containers such as a logger or backup manager.

## Important Evidence Or Examples

- One diagram shows the structural relationship between node, pod, and container.
- Another diagram shows a pod with a primary `postgres` container and support containers such as `logger` and `backup-manager`.
- The transcript explicitly argues that these support containers are only valuable while the primary database container exists, which is why the grouping makes sense.
- The existing manifest is reinterpreted through this lens as a one-container pod rather than a direct "run one container" instruction:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: client-pod
  labels:
    component: web
spec:
  containers:
    - name: client
      image: stephengrider/multi-client
      ports:
        - containerPort: 3000
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source makes the pod rationale clearer, but it still leaves open which higher-level object model is preferred once the course moves past the first local example.
