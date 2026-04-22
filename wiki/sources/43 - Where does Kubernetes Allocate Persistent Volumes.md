---
title: Where does Kubernetes Allocate Persistent Volumes
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/43 - Where does Kubernetes Allocate Persistent Volumes.md
source_date: 2026-04-20
tags:
  - kubernetes
  - persistentvolume
  - persistentvolumeclaim
  - storageclass
  - storage
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/41 - Claim Config files.md
  - wiki/sources/42 - Persistent Volume Access Modes.md
  - wiki/sources/44 - Designating a PVC in a POD Template.md
---

# Where does Kubernetes Allocate Persistent Volumes

## Metadata

- Source type: transcript with diagrams and command-line walkthrough
- Transcript quality: strong enough to preserve the default local `StorageClass` behavior, the `kubectl get storageclass` and `kubectl describe storageclass` inspection flow, and the local-versus-cloud storage-provisioner distinction
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-20 at 12.00.34 AM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 12.03.21 AM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 12.04.37 AM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 12.04.54 AM.png]]
- Derived notes:
  - `derived/notes/43 - Where does Kubernetes Allocate Persistent Volumes - Cheat Sheet.md`
  - `derived/notes/43 - Where does Kubernetes Allocate Persistent Volumes - Explainer.md`

## Summary

This source explains where Kubernetes gets storage from when a `PersistentVolumeClaim` is made. On a local machine, the default answer is usually simple: Kubernetes carves the storage out of the local host disk through the default `StorageClass`. The lesson demonstrates how to inspect that default with `kubectl get storageclass` and `kubectl describe storageclass`, then broadens the picture by showing that cloud environments have many possible storage provisioners, such as Google Cloud Persistent Disk, Azure File, Azure Disk, and AWS Block Store.

## Key Claims

- A `PersistentVolumeClaim` is effectively asking Kubernetes to find or create storage that matches the claim requirements.
- On a local machine, Kubernetes usually has very few storage choices and will commonly use the host machine's disk.
- The default local `StorageClass` can be inspected with `kubectl get storageclass`.
- The current environment shows a default `hostpath` storage class with `docker.io/hostpath` as the provisioner.
- `kubectl describe storageclass` shows more detail about the provisioner and its behavior.
- In cloud environments, Kubernetes can use very different storage provisioners depending on the platform.
- Cloud storage options include examples like Google Cloud Persistent Disk, Azure File, Azure Disk, and AWS Block Store.

## Important Evidence Or Examples

- The local-machine diagram shows Kubernetes taking a request for storage and carving out a slice of the host disk.
- The course demonstrates:

```bash
kubectl get storageclass
kubectl describe storageclass
```

- The captured `get storageclass` output shows:

```text
NAME                PROVISIONER          RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
hostpath (default)  docker.io/hostpath   Delete          Immediate           false                  29d
```

- The `describe storageclass` output shows the same default class and provisioner details, including:
  - `IsDefaultClass: Yes`
  - `Provisioner: docker.io/hostpath`
  - `ReclaimPolicy: Delete`
  - `VolumeBindingMode: Immediate`
- The cloud-provider diagram makes the contrast explicit:
  - local development often has one simple default storage source
  - cloud environments often have multiple storage backends to choose from

## Important Commands

```bash
kubectl get storageclass
kubectl describe storageclass
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript's spoken command phrasing is slightly sloppy in one place, but the screenshots make clear that the real inspection commands are about `storageclass`, not `kubectl` itself.
- The lesson shows the default storage-class behavior, but it still does not add a `storageClassName` field to the PVC manifest explicitly.
- The source explains where the storage comes from, but it still does not complete the final step of mounting that PVC into the Postgres pod template.
