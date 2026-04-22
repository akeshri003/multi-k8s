# Kubernetes Volumes Cheat Sheet

## Source

- Transcript: `raw/transcripts/38 -Kubernetes Volumes.md`
- Wiki source page: [[38 -Kubernetes Volumes]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 11.03.01 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.04.03 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.04.59 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.06.34 PM.png]]

## Core Idea

In Kubernetes, `Volume` means a specific pod-level storage thing.

That is not the same as the broad generic way people say “volume” in container discussions.

## The Important Distinction

- generic “volume”
  some way to store data outside the container
- Kubernetes `Volume`
  a pod-level storage object

## Why A Kubernetes `Volume` Is Not Enough For Postgres

- container restarts inside the same pod can still use the same `Volume`
- but if the pod dies, the `Volume` dies too
- that still risks database data loss

## What The Course Actually Wants

- `PersistentVolumeClaim`
- `PersistentVolume`

Not:

- plain Kubernetes `Volume`

## Retrieval Cues

- "Kubernetes volume is pod-level"
- "pod restart boundary matters"
- "good for container restart, not pod replacement"
- "PVC/PV are the real persistence targets"
