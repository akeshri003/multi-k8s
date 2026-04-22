---
title: Running Containers with Deployments
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/14 - Running Containers with Deployments.md
source_date: 2026-04-19
tags:
  - kubernetes
  - deployment
  - pod
  - pod-template
  - declarative
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/13 - Limitations in Config Updates.md
---

# Running Containers with Deployments

## Metadata

- Source type: transcript with embedded conceptual diagrams
- Transcript quality: strong enough to preserve the main object-model shift, the pod-versus-deployment comparison, and the pod-template explanation
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.21.21 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.23.11 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.23.26 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.26.27 PM.png]]
- Derived notes:
  - `derived/notes/14 - Running Containers with Deployments - Cheat Sheet.md`
  - `derived/notes/14 - Running Containers with Deployments - Explainer.md`

## Summary

This source introduces `Deployment` as the workaround for the limitations of direct pod updates. The lesson says that a deployment is a Kubernetes object that maintains a set of identical pods and keeps them in the correct configuration and runnable state. It contrasts direct `Pod` usage with `Deployment` usage, argues that serious development and production work should use deployments rather than bare pods, and introduces the idea of a pod template: a block of configuration inside the deployment that defines what each managed pod should look like.

## Key Claims

- Direct pod updates are too limited for ongoing application changes, which is why the lesson moves away from working with pods directly.
- A `Deployment` is a Kubernetes object that maintains a set of identical pods.
- That set can contain one pod or many pods.
- Every pod managed by a deployment is supposed to run the same configuration.
- A deployment constantly watches the pods it manages and tries to keep them running and correctly configured.
- If one of those pods crashes, the deployment will restart or recreate it.
- Pods are presented here as a development-time teaching tool or one-off mechanism, while deployments are presented as the normal way to run containers in both development and production.
- A deployment contains a pod template, which describes what each created pod should look like.
- The transcript claims that once a workload is managed through a deployment, changes to pod-level details such as the container port can be handled through the deployment instead of failing with the direct pod-update restriction.

## Important Evidence Or Examples

- The lesson starts by connecting directly to the previous error: direct pod updates failed because some pod fields cannot be changed after creation.
- The core new comparison is:
  - `Pod`: runs one tightly related container set; used directly only in simple or teaching cases
  - `Deployment`: manages one or more identical pods and is the standard path for serious work
- One diagram introduces the deployment object and its relationship to pods.
- Another diagram introduces the pod template and shows that the deployment uses that template to create pods with a specific image, port, and container name.
- The example pod template conceptually says each managed pod should:

```text
- run the multi-worker image
- expose port 3000
- use the client container definition
```

- The transcript then imagines changing the pod template from port `3000` to `9999`, and says the deployment would either update the managed pod or replace it with a new pod that matches the template.
- The source explicitly says that from this point forward the course will mostly stop working with direct pods and instead use deployments to run containers.

## Practical Model

The transcript does not yet paste a concrete deployment YAML manifest, but its conceptual model is:

```text
Deployment
  -> owns or manages a set of identical Pods
  -> each Pod is created from a shared pod template
```

That template includes pod-level details like:

```text
container name
image
container port
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source strongly suggests that deployments remove the painful direct-pod update limits, but it does not yet show the actual deployment manifest syntax or exact reconciliation behavior.
- The lesson still uses older terminology and simplified architecture language, so the pod-versus-deployment boundary may need more precise refinement later.
- The transcript claims deployments can handle changes like port updates by replacing or updating pods, but the exact mechanism is deferred to later material.

