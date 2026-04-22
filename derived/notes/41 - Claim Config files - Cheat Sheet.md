# Claim Config files Cheat Sheet

## Source

- Transcript: `raw/transcripts/41 - Claim Config files.md`
- Wiki source page: [[41 - Claim Config files]]

## Core Idea

This lesson creates the first real PVC manifest.

The PVC does not store data by itself.

It asks Kubernetes for storage that matches certain requirements.

## Main PVC YAML

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: database-persistent-volume-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```

## What Matters Most

- `kind: PersistentVolumeClaim`
- name: `database-persistent-volume-claim`
- access mode: `ReadWriteOnce`
- storage request: `2Gi`

## Retrieval Cues

- "PVC asks for storage"
- "`ReadWriteOnce` plus `2Gi`"
- "claim first, pod attachment later"
