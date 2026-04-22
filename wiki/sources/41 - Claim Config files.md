---
title: Claim Config files
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/41 - Claim Config files.md
source_date: 2026-04-20
tags:
  - kubernetes
  - persistentvolumeclaim
  - pvc
  - postgres
  - storage
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/40 - Persistent Volume vs Persistent Volume Claim.md
  - wiki/sources/44 - Designating a PVC in a POD Template.md
---

# Claim Config files

## Metadata

- Source type: transcript with embedded YAML walkthrough
- Transcript quality: strong enough to preserve the first concrete `PersistentVolumeClaim` manifest and the meaning of its core fields
- Embedded visuals:
  - None explicitly provided in the transcript or newly added assets
- Derived notes:
  - `derived/notes/41 - Claim Config files - Cheat Sheet.md`
  - `derived/notes/41 - Claim Config files - Explainer.md`

## Summary

This source creates the first real `PersistentVolumeClaim` manifest for the course. The claim asks Kubernetes for storage with one access mode, `ReadWriteOnce`, and a size request of `2Gi`. The lesson's main point is that this file does not create a pod or a database by itself. Instead, it defines a storage option that a pod template can later request and bind to.

## Key Claims

- A `PersistentVolumeClaim` is the request layer for storage, not the storage instance itself.
- The course's claim uses `apiVersion: v1`.
- The claim kind is `PersistentVolumeClaim`.
- The claim name is `database-persistent-volume-claim`.
- `accessModes` is an array, even when only one access mode is used.
- The course uses `ReadWriteOnce` for this Postgres example.
- The claim requests `2Gi` of storage.
- Kubernetes will try to satisfy this claim using an existing persistent volume or by creating one on demand.

## Important Evidence Or Examples

- The claim config introduced in the lesson is:

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

- The lesson explicitly frames the claim as an advertised storage option that a pod config can later reference.
- The transcript says the interesting new material starts inside `spec`, especially:
  - `accessModes`
  - `resources.requests.storage`
- The lesson treats `2Gi` as intentionally larger than needed for the simple demo database.

## Important Commands

- No shell command is central in this source.
- The lesson is focused on writing the PVC YAML itself.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson creates the claim YAML, but it still does not explain what `ReadWriteOnce` actually means in node-level terms.
- The claim now exists as a storage request, but the transcript still has not shown how that claim is attached to the Postgres pod template.
- The source writes the first PVC manifest, but it still relies on later lessons to explain defaults like storage class selection.
