# Volume vs Persistent Volume Cheat Sheet

## Source

- Transcript: `raw/transcripts/39 - Volume vs Persistent Volume.md`
- Wiki source page: [[39 - Volume vs Persistent Volume]]

## Key Visual

- ![[raw/assets/Screenshot 2026-04-19 at 11.08.28 PM.png]]

## Core Idea

The whole lesson is about one question:

what survives only a container restart, and what survives a full pod replacement?

## The Fast Comparison

- `Volume`
  - tied to the pod
  - survives container restart inside the same pod
  - does not survive pod deletion or pod recreation
- `PersistentVolume`
  - outside the pod
  - survives container restart
  - survives pod recreation
  - lasts until someone intentionally deletes it

## Why This Matters For Postgres

- Postgres data must survive more than a container restart.
- In Kubernetes, pods can be deleted and recreated.
- So a plain pod-level `Volume` is still not enough.
- The course is moving toward `PersistentVolume` because it survives the failure boundary that matters for a database.

## Retrieval Cues

- "`Volume` belongs to the pod"
- "`PersistentVolume` lives outside the pod"
- "container restart is not the real test"
- "pod replacement is the real durability test"
