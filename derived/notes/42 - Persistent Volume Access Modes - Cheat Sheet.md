# Persistent Volume Access Modes Cheat Sheet

## Source

- Transcript: `raw/transcripts/42 - Persistent Volume Access Modes.md`
- Wiki source page: [[42 - Persistent Volume Access Modes]]

## Key Visual

- ![[raw/assets/Screenshot 2026-04-19 at 11.57.16 PM.png]]

## Core Idea

The PVC is asking Kubernetes for storage with specific rules.

The most important rule here is the access mode.

## Access Modes

- `ReadWriteOnce`
  - one node can read and write
- `ReadOnlyMany`
  - many nodes can read
- `ReadWriteMany`
  - many nodes can read and write

## Course Example

```yaml
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```

## What The Current PVC Is Asking For

- one storage instance
- `2Gi` capacity
- usable by one node for both reading and writing

## Retrieval Cues

- "claim defines requirements"
- "`ReadWriteOnce` = one node reads and writes"
- "`2Gi` is just the size request"
