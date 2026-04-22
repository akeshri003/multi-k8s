# Limitations in Config Updates Cheat Sheet

## Source

- Transcript: `raw/transcripts/13 - Limitations in Config Updates.md`
- Wiki source page: [[13 - Limitations in Config Updates]]
- Key visual:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.17.34 PM.png]]

## Core Idea

Declarative updates are still the right workflow, but not every pod field can be changed in place.

## Example In This Lesson

The lesson changes:

```text
containerPort: 3000
```

to:

```text
containerPort: 9999
```

and then runs:

```bash
kubectl apply -f client-pod.yaml
```

## What Happens

Kubernetes rejects the update.

Main error:

```text
The Pod "client-pod" is invalid: spec: Forbidden: pod updates may not change fields other than ...
```

## What This Means

- changing the image was allowed in the last lesson
- changing `containerPort` is not allowed here
- some pod fields are mutable
- many pod fields are effectively immutable after creation

## Allowed Fields Mentioned In The Error

The transcript's error explicitly allows:

- `spec.containers[*].image`
- `spec.initContainers[*].image`
- `spec.activeDeadlineSeconds`
- `spec.tolerations` with additions only
- a limited case for `spec.terminationGracePeriodSeconds`

## Most Important Evidence

```diff
- ContainerPort: 3000
+ ContainerPort: 9999
```

That diff is the exact rejected change.

## Retrieval Cues

- "not every declarative change is allowed in place"
- "image change worked, port change failed"
- "pod updates have strict limits"
- "this points toward a higher-level object model"

