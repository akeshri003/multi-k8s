---
title: Designating a PVC in a POD Template
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/44 - Designating a PVC in a POD Template.md
source_date: 2026-04-20
tags:
  - kubernetes
  - postgres
  - persistentvolumeclaim
  - deployment
  - volumes
  - volumemounts
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/36 - Last set of config.md
  - wiki/sources/41 - Claim Config files.md
  - wiki/sources/43 - Where does Kubernetes Allocate Persistent Volumes.md
---

# Designating a PVC in a POD Template

## Metadata

- Source type: transcript with embedded YAML walkthrough
- Transcript quality: strong enough to preserve how a deployment template references a PVC, how that volume is mounted into the Postgres container, and why `subPath: postgres` is added for this specific image
- Embedded visuals:
  - None explicitly provided in the transcript or newly added assets
- Derived notes:
  - `derived/notes/44 - Designating a PVC in a POD Template - Cheat Sheet.md`
  - `derived/notes/44 - Designating a PVC in a POD Template - Explainer.md`

## Summary

This source finishes the first real persistent-storage wiring for the course. It updates the `postgres-deployment` pod template to request storage through a `volumes` entry backed by the `database-persistent-volume-claim`, then mounts that storage into the Postgres container through `volumeMounts`. The lesson also adds `subPath: postgres`, which the instructor treats as a Postgres-specific workaround so the image writes data into a nested folder inside the mounted storage.

## Key Claims

- The pod template inside `postgres-deployment` is the place where the PVC gets attached.
- A `volumes` entry in the pod spec can reference a `persistentVolumeClaim`.
- The claim name used here is `database-persistent-volume-claim`.
- A `volumeMounts` entry in the container spec makes that allocated storage available inside the container filesystem.
- The `name` in `volumeMounts` must match the `name` defined in the `volumes` entry.
- The Postgres data directory to mount is `/var/lib/postgresql/data`.
- The source adds `subPath: postgres` for this Postgres example so the data is stored inside a nested folder on the persistent storage.

## Important Evidence Or Examples

- The normalized deployment fragment from the spoken walkthrough is:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: postgres
  template:
    metadata:
      labels:
        component: postgres
    spec:
      volumes:
        - name: postgres-storage
          persistentVolumeClaim:
            claimName: database-persistent-volume-claim
      containers:
        - name: postgres
          image: postgres
          ports:
            - containerPort: 5432
          volumeMounts:
            - name: postgres-storage
              mountPath: /var/lib/postgresql/data
              subPath: postgres
```

- The lesson explains the wiring in three steps:
  - the PVC defines the storage requirements
  - the pod `volumes` section asks to use storage that satisfies that claim
  - the container `volumeMounts` section exposes that storage at a concrete filesystem path
- The transcript explicitly says the shared `name` field is the link between the `volumes` entry and the `volumeMounts` entry.
- The raw transcript YAML contains transcription problems such as `matchingLabels` and `PersistentVolumeClaim` capitalization, so the normalized block above follows the spoken explanation rather than the pasted text literally.

## Important Commands

- No shell command is central in this source.
- The lesson is focused on updating the deployment YAML.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The lesson shows the full PVC wiring in YAML, but it still stops before applying the updated config and verifying that the Postgres pod really mounts the storage successfully.
- The source gives a high-level reason for `subPath: postgres`, but the exact Postgres image behavior is intentionally simplified.
- The new storage path is now described in config, but the later runtime verification and inspection steps still have to prove that the data actually persists.
