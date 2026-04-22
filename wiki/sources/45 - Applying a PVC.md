---
title: Applying a PVC
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/45 - Applying a PVC.md
source_date: 2026-04-20
tags:
  - kubernetes
  - persistentvolumeclaim
  - persistentvolume
  - postgres
  - verification
  - storage
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/41 - Claim Config files.md
  - wiki/sources/44 - Designating a PVC in a POD Template.md
---

# Applying a PVC

## Metadata

- Source type: transcript with command-line verification walkthrough
- Transcript quality: strong enough to preserve the apply flow and the first practical inspection commands for `PersistentVolume` and `PersistentVolumeClaim`
- Embedded visuals:
  - None explicitly provided in the transcript or newly added assets
- Derived notes:
  - `derived/notes/45 - Applying a PVC - Cheat Sheet.md`
  - `derived/notes/45 - Applying a PVC - Explainer.md`

## Summary

This source applies the new PVC-backed Postgres configuration to the cluster and verifies that the main storage objects now exist. The lesson reapplies the full `k8s/` directory, checks that the Postgres pod restarted and is running, then uses `kubectl get pv` and `kubectl get pvc` to show that a real persistent volume has been created and bound to the `database-persistent-volume-claim`. The source also makes an important limit explicit: the storage layer is now configured, but the course still cannot prove real data persistence until the server and worker are wired to write data into Postgres.

## Key Claims

- After adding the PVC config and mounting it into the deployment, the full `k8s/` directory should be re-applied.
- Reapplying the config creates the claim and reconfigures the Postgres deployment.
- `kubectl get pods` should show the restarted Postgres pod running.
- `kubectl get pv` shows the actual persistent volume instances in the cluster.
- The persistent volume created for this setup is expected to be bound to `database-persistent-volume-claim`.
- `kubectl get pvc` shows the claims that have been created.
- A `PersistentVolumeClaim` is still the request/advertisement layer, while the `PersistentVolume` is the actual storage instance that satisfies it.
- The lesson does not yet prove real data durability because the app is not yet writing meaningful data into Postgres.

## Important Evidence Or Examples

- The verification flow in the lesson is:

```bash
kubectl apply -f k8s
kubectl get pods
kubectl get pv
kubectl get pvc
```

- The transcript explains the expected meanings of the results:
  - `kubectl get pods` confirms the Postgres pod restarted and is running
  - `kubectl get pv` shows the actual storage object that was created
  - `kubectl get pvc` shows the claim object that requested that storage
- The source explicitly says the `PersistentVolume` entry should show the claim association on the far right and that the status should be `Bound`.
- The lesson frames `pv` and `pvc` as the practical way to see the difference between:
  - actual storage instance
  - storage request object

## Important Commands

```bash
kubectl apply -f k8s
kubectl get pods
kubectl get pv
kubectl get pvc
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The storage objects are now visible and bound, but the lesson still does not prove persistence by creating and recovering real database data.
- The source explicitly says the next missing piece is environment-variable wiring for the server and worker so they can actually talk to Redis and Postgres.
- This lesson verifies object existence and binding status, but it stops before any deeper inspection of mounts inside the Postgres container.
