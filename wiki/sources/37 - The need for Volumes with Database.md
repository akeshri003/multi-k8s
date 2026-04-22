---
title: The need for Volumes with Database
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/37 - The need for Volumes with Database.md
source_date: 2026-04-19
tags:
  - kubernetes
  - postgres
  - volume
  - pvc
  - storage
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/36 - Last set of config.md
---

# The need for Volumes with Database

## Metadata

- Source type: transcript with embedded diagrams and conceptual storage explanation
- Transcript quality: strong enough to preserve the reason databases need persistence, the host-volume mental model, and the warning against sharing one database volume across arbitrarily duplicated replicas
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 10.55.14 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 10.55.33 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 10.57.37 PM.png]]
- Derived notes:
  - `derived/notes/37 - The need for Volumes with Database - Cheat Sheet.md`
  - `derived/notes/37 - The need for Volumes with Database - Explainer.md`

## Summary

This source explains why Postgres needs persistence at all. If Postgres stores data only inside the container filesystem, that data disappears when the pod is replaced. The lesson reintroduces the generic container idea of a volume as storage outside the container’s own filesystem, then uses Postgres to show why the database should write to persistent storage instead of to the ephemeral container filesystem. It also adds an important warning: simply increasing the replica count and letting multiple database instances share the same storage without proper coordination would be dangerous.

## Key Claims

- Postgres writes data to a filesystem.
- If that filesystem lives only inside the container, the data is lost when the pod is replaced.
- Databases need some form of persistent storage outside the container’s own isolated filesystem.
- A volume solves the persistence problem by letting the database write to storage outside the container itself.
- A replacement pod can reconnect to the same outside storage and keep the old data.
- Arbitrarily increasing Postgres replicas to multiple copies sharing the same storage would be unsafe without proper database-specific coordination.
- This lesson is still conceptual; it is preparing for the PVC explanation, not yet implementing it.

## Important Evidence Or Examples

- One diagram highlights the final architecture and shows the `Persistent Volume Claim` attached to the Postgres side of the system.
- Another diagram shows the naive state:
  - a Postgres deployment
  - one pod
  - one Postgres container
  - data stored only in the container filesystem
- A third diagram shows the failure case:
  - old pod crashes
  - deployment creates a new pod
  - no data carries over
- The transcript explicitly says that if Postgres stores data only inside the container filesystem, a recreated pod starts with none of the previous data.
- The lesson also warns that simply dialing `replicas` from `1` to `2` for Postgres would be a mistake if both copies tried to use the same storage without proper clustering/coordination.

## Important Commands

- No shell command is central in this source.
- The lesson is conceptual and diagram-driven.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The lesson reuses the general container idea of a volume, but it still has not yet clarified the Kubernetes-specific distinction between a `Volume`, a `PersistentVolume`, and a `PersistentVolumeClaim`.
- The source makes the need for persistence clear, but it still does not show the actual Kubernetes config that will implement that persistence.
- The transcript warns against naive Postgres scaling, but it does not yet explain how proper high-availability database setups are actually coordinated in Kubernetes.
