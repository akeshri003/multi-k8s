# Designating a PVC in a POD Template Cheat Sheet

## Source

- Transcript: `raw/transcripts/44 - Designating a PVC in a POD Template.md`
- Wiki source page: [[44 - Designating a PVC in a POD Template]]

## Core Idea

This lesson connects the storage claim to the Postgres pod.

The flow is:

1. PVC defines what storage is needed
2. pod `volumes` asks for that claim
3. container `volumeMounts` exposes it inside Postgres

## Main Deployment Fragment

```yaml
spec:
  template:
    spec:
      volumes:
        - name: postgres-storage
          persistentVolumeClaim:
            claimName: database-persistent-volume-claim
      containers:
        - name: postgres
          image: postgres
          ports:
            - containerPort: 5432
          volumeMounts:
            - name: postgres-storage
              mountPath: /var/lib/postgresql/data
              subPath: postgres
```

## What Matters Most

- `claimName` points to the PVC
- `volumes.name` and `volumeMounts.name` must match
- `mountPath` is where Postgres sees the storage
- `subPath: postgres` is the Postgres-specific extra setting

## Retrieval Cues

- "PVC in pod template"
- "`volumes` allocates, `volumeMounts` exposes"
- "same name links the two sections"
