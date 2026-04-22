---
title: Persistent Volume vs Persistent Volume Claim
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/40 - Persistent Volume vs Persistent Volume Claim.md
source_date: 2026-04-19
tags:
  - kubernetes
  - persistentvolume
  - persistentvolumeclaim
  - storage
  - postgres
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/38 -Kubernetes Volumes.md
  - wiki/sources/39 - Volume vs Persistent Volume.md
---

# Persistent Volume vs Persistent Volume Claim

## Metadata

- Source type: transcript with analogy-driven comparison diagrams
- Transcript quality: strong enough to preserve the course's PV/PVC analogy, the request-versus-resource distinction, and the static-versus-dynamic provisioning idea
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 11.11.45 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 11.12.46 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 11.15.30 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 11.17.17 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 11.18.56 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 11.19.33 PM.png]]
- Derived notes:
  - `derived/notes/40 - Persistent Volume vs Persistent Volume Claim - Cheat Sheet.md`
  - `derived/notes/40 - Persistent Volume vs Persistent Volume Claim - Explainer.md`

## Summary

This source introduces the course's mental model for `PersistentVolumeClaim` versus `PersistentVolume`. A `PersistentVolumeClaim` is the request layer: it describes the storage option a pod wants. A `PersistentVolume` is the actual storage resource that can satisfy that request. The lesson also adds the provisioning distinction: some persistent volumes are created ahead of time and are ready immediately, while others are created only when a matching claim is made.

## Key Claims

- A `PersistentVolumeClaim` is not the actual storage itself.
- A `PersistentVolumeClaim` is best understood as a request or advertised option for storage.
- A `PersistentVolume` is the actual underlying storage resource.
- Some `PersistentVolume` instances may already exist ahead of time.
- A pre-created persistent volume is described here as statically provisioned storage.
- Some persistent volumes are created only when a matching request arrives.
- An on-demand persistent volume is described here as dynamically provisioned storage.
- From the developer's point of view, the important step is making the right claim; Kubernetes then tries to satisfy that claim using existing storage or by creating storage on demand.

## Important Evidence Or Examples

- The lesson uses a computer-store analogy:
  - the billboard advertises `500 GB` and `1 TB` options
  - the customer asks for one option
  - the salesperson either finds an already-existing drive or gets one made on demand
- In the Kubernetes mapping:
  - the billboard corresponds to `PersistentVolumeClaim` options
  - the actual hard drives correspond to `PersistentVolume` resources
  - Kubernetes plays the salesperson role that matches a request to real storage
- The lesson explicitly distinguishes two persistent-volume paths:
  - statically provisioned: storage already exists
  - dynamically provisioned: storage is created after the request arrives
- The lesson's main point is operational:
  - developers ask for storage through claims
  - Kubernetes satisfies that claim with real persistent volume resources

## Important Commands

- No shell command is central in this source.
- The lesson is conceptual and analogy-driven.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The lesson gives a useful mental model, but it still does not show the actual YAML shape of a `PersistentVolumeClaim`.
- The analogy makes the request-versus-resource distinction clear, but it still leaves storage class details, access modes, and binding rules for later.
- The course has now explained why Postgres needs durable storage and how the request/resource split works, but the actual Postgres PVC wiring still has not appeared yet.
