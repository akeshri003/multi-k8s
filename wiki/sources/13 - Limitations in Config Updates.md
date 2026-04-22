---
title: Limitations in Config Updates
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/13 - Limitations in Config Updates.md
source_date: 2026-04-19
tags:
  - kubernetes
  - pod
  - declarative
  - update
  - immutability
  - kubectl
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/11 - Updating Existing Objects.md
  - wiki/sources/12 - Declarative Updates in Action.md
---

# Limitations in Config Updates

## Metadata

- Source type: transcript with an embedded screenshot, command example, and error output
- Transcript quality: strong enough to preserve both the concrete failed update attempt and the rule Kubernetes enforces for pod updates
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.17.34 PM.png]]
- Derived notes:
  - `derived/notes/13 - Limitations in Config Updates - Cheat Sheet.md`
  - `derived/notes/13 - Limitations in Config Updates - Explainer.md`

## Summary

This source introduces an important limit to the earlier declarative-update story. After successfully changing the pod image in the previous lesson, the transcript now changes `containerPort` from `3000` to `9999` and tries to re-apply the pod manifest. Kubernetes rejects the update and returns a long validation error saying that pod updates are only allowed to change a small set of fields, such as container images and a few other specific properties. The lesson's point is that declarative updates are still the preferred workflow, but not every field on a pod can be changed in place.

## Key Claims

- A pod manifest can be re-applied declaratively, but only some fields are allowed to change in place.
- Changing `spec.containers[*].image` is allowed, which is why the earlier image update succeeded.
- Changing `containerPort` on an existing pod is rejected.
- The transcript says Kubernetes allows only a narrow set of in-place pod updates, including container images, init container images, `activeDeadlineSeconds`, additions to existing `tolerations`, and a limited case for `terminationGracePeriodSeconds`.
- The error message is the important evidence here; the restriction is enforced by Kubernetes itself, not just by the course's advice.
- The earlier rule "change the config and re-apply it" still exists, but this lesson shows that the rule has object-specific limits.

## Important Evidence Or Examples

- The lesson makes a small but intentional change in `client-pod.yaml`:

```text
from: containerPort: 3000
to:   containerPort: 9999
```

- The apply command is:

```bash
kubectl apply -f client-pod.yaml
```

- Kubernetes rejects it with:

```text
The Pod "client-pod" is invalid: spec: Forbidden: pod updates may not change fields other than `spec.containers[*].image`, `spec.initContainers[*].image`, `spec.activeDeadlineSeconds`, `spec.tolerations` (only additions to existing tolerations) or `spec.terminationGracePeriodSeconds` (allow it to be set to 1 if it was previously negative)
```

- The diff shown in the error output highlights the exact rejected change:

```diff
- ContainerPort: 3000
+ ContainerPort: 9999
```

- The transcript contrasts this failure with the previous successful image update to emphasize that not all pod fields are equally mutable.

## Important Commands

The transcript's concrete command for the failed update is:

```bash
kubectl apply -f client-pod.yaml
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source shows that direct pod updates are limited, which strengthens the open question of what higher-level Kubernetes object model should be used for more realistic ongoing change management.
- The lesson promises a workaround but does not explain it yet, so this transcript leaves the problem intentionally unresolved.
- The course still teaches pod-level examples, but this immutability rule suggests that direct pod editing is not the general long-term production workflow.

