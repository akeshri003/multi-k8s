---
title: Persistent Volume Access Modes
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/42 - Persistent Volume Access Modes.md
source_date: 2026-04-20
tags:
  - kubernetes
  - persistentvolumeclaim
  - persistentvolume
  - accessmodes
  - storage
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/41 - Claim Config files.md
  - wiki/sources/43 - Where does Kubernetes Allocate Persistent Volumes.md
---

# Persistent Volume Access Modes

## Metadata

- Source type: transcript with PVC YAML recap and one comparison diagram
- Transcript quality: strong enough to preserve the meaning of the three PVC access modes and the exact `2Gi` storage request interpretation used in the course
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 11.57.16 PM.png]]
- Derived notes:
  - `derived/notes/42 - Persistent Volume Access Modes - Cheat Sheet.md`
  - `derived/notes/42 - Persistent Volume Access Modes - Explainer.md`

## Summary

This source explains the meaning of the two fields that were just added to the `PersistentVolumeClaim` spec: `accessModes` and `resources.requests.storage`. The main idea is that the claim is a request contract. `ReadWriteOnce` asks Kubernetes for storage that can be read from and written to by a single node at a time. The lesson also briefly introduces the other two access modes, `ReadOnlyMany` and `ReadWriteMany`, and clarifies that the course's example is simply asking for `2Gi` of storage capacity.

## Key Claims

- A volume claim is not an actual storage instance.
- The PVC spec tells Kubernetes what kind of persistent storage it must find or create.
- `ReadWriteOnce` means a single node can read from and write to the storage.
- `ReadOnlyMany` means multiple nodes can read from the storage at the same time.
- `ReadWriteMany` means multiple nodes can both read from and write to the storage at the same time.
- The course's `storage: 2Gi` field is the requested storage capacity for the claim.
- The lesson treats `2Gi` as much larger than the demo Postgres database really needs.

## Important Evidence Or Examples

- The source recaps the same PVC manifest:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: database-persistent-volume-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```

- The access-mode diagram maps:
  - `ReadWriteOnce` -> one node can read and write
  - `ReadOnlyMany` -> many nodes can read
  - `ReadWriteMany` -> many nodes can read and write
- The lesson explicitly frames the claim as a requirement that Kubernetes must satisfy with a compatible storage instance.

## Important Commands

- No shell command is central in this source.
- The lesson is still conceptual, even though it discusses YAML fields.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The lesson explains what each access mode means, but it still does not say which storage backends actually support each mode.
- The source clarifies what the claim asks for, but it still has not shown how Kubernetes decides where to create that storage on a local machine or in the cloud.
- The PVC fields are now easier to interpret, but the pod template still has not been updated to mount the claim.
