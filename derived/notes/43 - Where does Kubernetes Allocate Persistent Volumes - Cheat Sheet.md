# Where does Kubernetes Allocate Persistent Volumes Cheat Sheet

## Source

- Transcript: `raw/transcripts/43 - Where does Kubernetes Allocate Persistent Volumes.md`
- Wiki source page: [[43 - Where does Kubernetes Allocate Persistent Volumes]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 12.00.34 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.03.21 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.04.37 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.04.54 AM.png]]

## Core Idea

When a PVC asks for storage, Kubernetes has to decide where that storage actually comes from.

## Local Default

- on a local machine, the default is often the host disk
- Kubernetes uses the default `StorageClass`
- in this setup, that default is `hostpath`

## Commands To Inspect It

```bash
kubectl get storageclass
kubectl describe storageclass
```

## What The Captured Output Shows

- default class: `hostpath`
- provisioner: `docker.io/hostpath`
- reclaim policy: `Delete`
- binding mode: `Immediate`

## Cloud Contrast

- local laptop: usually one simple storage source
- cloud: many storage providers and provisioners
- examples:
  - Google Cloud Persistent Disk
  - Azure File
  - Azure Disk
  - AWS Block Store

## Retrieval Cues

- "PVC asks, StorageClass decides how"
- "local default = host disk slice"
- "`kubectl get storageclass`"
