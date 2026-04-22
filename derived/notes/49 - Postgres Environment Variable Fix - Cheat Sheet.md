# Postgres Environment Variable Fix Cheat Sheet

## Source

- Transcript: `raw/transcripts/49 - Postgres Environment Variable Fix.md`
- Wiki source page: [[49 - Postgres Environment Variable Fix]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 11.37.52 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 11.38.06 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 11.38.34 AM.png]]

## Core Idea

This lesson makes the `pgpassword` secret usable inside the deployments.

## Server Secret Env Entry

```yaml
- name: PGPASSWORD
  valueFrom:
    secretKeyRef:
      name: pgpassword
      key: PGPASSWORD
```

## Postgres Secret Env Entry

```yaml
env:
  - name: POSTGRES_PASSWORD
    valueFrom:
      secretKeyRef:
        name: pgpassword
        key: PGPASSWORD
```

## Main Command Flow

```bash
kubectl create secret generic pgpassword --from-literal PGPASSWORD=postgres
kubectl get secrets
kubectl apply -f k8s
```

## What Happens

- secret is created
- deployments are updated to read from the secret
- `kubectl apply` fails with `cannot convert int64 to string`

## Retrieval Cues

- "use `valueFrom`, not `value`"
- "server reads `PGPASSWORD`"
- "Postgres reads `POSTGRES_PASSWORD`"
- "next problem is YAML string typing"
