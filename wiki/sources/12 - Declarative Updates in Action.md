---
title: Declarative Updates in Action
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/12 - Declarative Updates in Action.md
source_date: 2026-04-19
tags:
  - kubernetes
  - declarative
  - kubectl
  - pod
  - describe
  - update
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/11 - Updating Existing Objects.md
  - wiki/analysis/kubernetes-object-identity.md
---

# Declarative Updates in Action

## Metadata

- Source type: transcript with embedded screenshots, terminal output, and a practical verification walkthrough
- Transcript quality: strong enough to preserve both the exact `kubectl` flow and the verification logic used to prove that the existing pod was updated in place
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.04.42 PM.png]]
- Referenced visual currently outside canonical `raw/` storage:
  - ![[Screenshot 2026-04-19 at 3.05.02 PM.png]]
- Derived notes:
  - `derived/notes/12 - Declarative Updates in Action - Cheat Sheet.md`
  - `derived/notes/12 - Declarative Updates in Action - Explainer.md`

## Summary

This source demonstrates the declarative update workflow in practice. The lesson edits the existing `client-pod.yaml` file to change the image from `stephengrider/multi-client` to `stephengrider/multi-worker`, re-applies the file with `kubectl apply -f client-pod.yaml`, and then verifies that Kubernetes updated the existing pod rather than creating a second one. It uses `kubectl get pods`, `docker ps`, and especially `kubectl describe pod client-pod` to prove that the pod kept the same identity while its container definition changed and the pod restarted with the new image.

## Key Claims

- Declarative updates follow the same basic formula every time: change the config file, apply it again, then verify the result.
- Re-applying the modified pod manifest returns `pod/client-pod configured`, which the lesson treats as the signal that Kubernetes updated the existing object's configuration.
- After the update, there is still only one pod named `client-pod`, which supports the lesson's claim that the object was updated in place rather than duplicated.
- `kubectl describe` is introduced as the detailed inspection command for a specific object.
- The `describe` output proves that the container image changed from `stephengrider/multi-client` to `stephengrider/multi-worker`.
- The event stream in `describe` is presented as especially useful because it records lifecycle events such as restarts and image pulls.
- The lesson continues to teach the simplified rule that keeping `name` and `kind` stable is what makes the config update target the existing object.

## Important Evidence Or Examples

- The transcript first shows the pre-update state:

```bash
kubectl get pods
kubectl get services
docker ps
```

with one running `client-pod`, one `client-node-port` service, and a Docker container using the `stephengrider/multi-client` image.

- The declarative update step is:

```bash
kubectl apply -f client-pod.yaml
```

and the recorded result is:

```text
pod/client-pod configured
```

- The post-update `kubectl get pods` output still shows a single `client-pod`, but now with a restart count, which the lesson interprets as evidence that the existing pod was restarted with a changed container definition.
- The core verification step is:

```bash
kubectl describe pod client-pod
```

- The `describe` output shows:
  - `Image: stephengrider/multi-worker`
  - `Last State: Terminated`
  - `Restart Count: 1`
  - lifecycle events such as:
    - `Container client definition changed, will be restarted`
    - `Pulling image "stephengrider/multi-worker"`
    - `Successfully pulled image "stephengrider/multi-worker"`

- The lesson explicitly notes that the `multi-worker` container may be unhappy without Redis, but says the goal here is only to prove the image update, not to make the full app healthy yet.

## Important Commands

The transcript's practical command sequence is:

```bash
kubectl get pods
kubectl get services
docker ps
kubectl apply -f client-pod.yaml
kubectl get pods
kubectl describe pod client-pod
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript uses restart count and `describe` output as proof of an in-place update, but it still operates inside the simplified pod-level teaching model rather than the more common higher-level production workflow.
- One of the lesson's screenshots is not yet in canonical `raw/assets/` storage, so the source page preserves that evidence with a storage caveat.
- The transcript reinforces `name + kind` as the update rule, but the fuller Kubernetes identity model in [[kubernetes-object-identity]] remains more precise than the teaching simplification used here.

