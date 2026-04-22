---
title: Scaling and Chaning Deployments
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/19 - Scaling and Chaning Deployments.md
source_date: 2026-04-19
tags:
  - kubernetes
  - deployment
  - scaling
  - rollout
  - replicas
  - kubectl
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/15 - Deployment Configuration files..md
  - wiki/sources/17 - Applying a  Deployment.md
  - wiki/sources/18 - Why use Services?.md
---

# Scaling and Chaning Deployments

## Metadata

- Source type: transcript with command sequences and deployment-status walkthrough
- Transcript quality: strong enough to preserve the deployment-update workflow, scaling behavior, and rollout-status interpretation
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.41.19 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.41.34 PM.png]]
- Derived notes:
  - `derived/notes/19 - Scaling and Chaning Deployments - Cheat Sheet.md`
  - `derived/notes/19 - Scaling and Chaning Deployments - Explainer.md`

## Summary

This source shows why deployments are much more flexible than direct pod configs. It changes the deployment's pod template, re-applies the manifest, and shows that Kubernetes replaces the old pod with a new one rather than trying to mutate the existing pod in place. It then scales `replicas` from `1` to `5`, shows that five pods appear, and finally changes the container image so the deployment performs a rolling-style replacement where old and new pods briefly coexist. The lesson also gives the first concrete example of `kubectl get deployments` showing temporary numbers such as `current` being larger than `desired` during an update.

## Key Claims

- Changing the pod template inside a deployment causes Kubernetes to recreate pods so they match the new template.
- Re-applying the deployment manifest reports `configured`, which the lesson uses as evidence that the existing deployment was updated rather than recreated from scratch.
- `kubectl get pods` can reveal that a pod was replaced because the new pod has a very young age.
- `kubectl describe pods` can be used to verify that the new pod is running with the latest template values.
- Increasing `replicas` from `1` to `5` makes the deployment create five separate pods from the same template.
- Changing the deployment image from `multi-client` to `multi-worker` triggers a rollout where old and new pods briefly exist together.
- During that rollout, `desired`, `current`, `up-to-date`, and `available` can temporarily differ in meaningful ways.

## Important Evidence Or Examples

- The transcript changes the deployment's container port away from `3000`, then applies the manifest again:

```bash
kubectl apply -f client-deployment.yaml
kubectl get deployments
kubectl get pods
kubectl describe pods
```

- The lesson interprets a very small pod age, such as `26 seconds`, as evidence that the original pod was deleted and recreated from the new template.
- The transcript says `kubectl describe pods` confirms that the recreated pod now shows port `999`.
- The transcript then scales the deployment from one replica to five and re-applies the manifest.
- After scaling, `kubectl get pods` is described as showing five distinct pods, each running the same application image from the deployment template.
- The lesson then changes the image from `multi-client` to `multi-worker`, applies the manifest again, and quickly checks deployment status.
- The important observed deployment state is:
  - `desired = 5`
  - `current = 7`
  - `up-to-date = 4`
  - `available = 4`
- The lesson interprets that snapshot as four new pods already updated to the newest spec, plus three older pods that still need to be cleaned up.

## Important Commands

The transcript's main command sequence is:

```bash
kubectl apply -f client-deployment.yaml
kubectl get deployments
kubectl get pods
kubectl describe pods
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript says the changed port becomes `99999` once and then repeatedly reasons about port `999`, so the exact intended replacement value is internally inconsistent.
- The source explains rollout behavior through observed `desired/current/up-to-date/available` numbers, but it does not yet explain the lower-level rollout strategy rules that allow `current` to temporarily exceed `desired`.
- The lesson says browser access will stop working after the port change, but the surrounding source base still treats local connectivity details as runtime-specific and partly simplified.
