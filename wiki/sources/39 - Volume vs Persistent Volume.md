---
title: Volume vs Persistent Volume
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/39 - Volume vs Persistent Volume.md
source_date: 2026-04-19
tags:
  - kubernetes
  - volume
  - persistentvolume
  - postgres
  - storage
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/37 - The need for Volumes with Database.md
  - wiki/sources/38 -Kubernetes Volumes.md
  - wiki/sources/40 - Persistent Volume vs Persistent Volume Claim.md
---

# Volume vs Persistent Volume

## Metadata

- Source type: transcript with one comparison diagram
- Transcript quality: strong enough to preserve the lifecycle difference between pod-level `Volume` storage and external `PersistentVolume` storage
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 11.08.28 PM.png]]
- Derived notes:
  - `derived/notes/39 - Volume vs Persistent Volume - Cheat Sheet.md`
  - `derived/notes/39 - Volume vs Persistent Volume - Explainer.md`

## Summary

This source sharpens the storage boundary introduced in the previous lessons. A Kubernetes `Volume` is tied to the pod lifecycle, so it can survive a container restart inside the same pod but it disappears if the pod itself is destroyed. A `PersistentVolume`, by contrast, sits outside any specific pod. That means both container recreation and pod recreation can happen without losing the stored data, which is why the course treats `PersistentVolume` as the right long-lived direction for Postgres.

## Key Claims

- A plain Kubernetes `Volume` is tied to the lifecycle of a specific pod.
- A pod-level `Volume` survives container replacement inside the same pod.
- A pod-level `Volume` is lost if the entire pod is deleted or recreated.
- A `PersistentVolume` is not tied to any specific pod or any specific container.
- A `PersistentVolume` can survive both container recreation and pod recreation.
- `PersistentVolume` is the storage model that matches the database durability problem more closely than a plain pod-level `Volume`.
- A `PersistentVolume` remains until an administrator or developer explicitly removes it.

## Important Evidence Or Examples

- The left side of the diagram shows:
  - a Postgres deployment
  - a pod-level `Volume`
  - a container crash that does not destroy the `Volume`
- That same left-side example also shows the failure boundary:
  - if the entire pod goes away
  - the attached `Volume` goes away too
- The right side of the diagram shows:
  - a Postgres pod
  - an external `PersistentVolume`
  - the pod being replaced while the storage remains
- The main comparison is lifecycle-based:
  - `Volume` follows the pod lifecycle
  - `PersistentVolume` outlives the pod lifecycle

## Important Commands

- No shell command is central in this source.
- The lesson is conceptual and diagram-driven.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson now makes the `Volume` versus `PersistentVolume` boundary clear, but it still does not show the actual object that requests or binds that storage from a pod config.
- The lesson explains what kind of storage Postgres needs, but it still stops short of the actual YAML needed to attach persistent storage to `postgres-deployment`.
- The next missing concept is no longer "why persistence matters" but "how a pod actually asks for a persistent volume in Kubernetes terms."
