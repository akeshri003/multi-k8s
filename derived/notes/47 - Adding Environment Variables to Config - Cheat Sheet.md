# Adding Environment Variables to Config Cheat Sheet

## Source

- Transcript: `raw/transcripts/47 - Adding Environment Variables to Config.md`
- Wiki source page: [[47 - Adding Environment Variables to Config]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 11.04.40 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 11.07.32 AM.png]]

## Core Idea

This lesson puts the first real `env` blocks into the worker and server deployments.

## Worker Env Block

```yaml
env:
  - name: REDIS_HOST
    value: redis-cluster-ip-service
  - name: REDIS_PORT
    value: "6379"
```

## Server Env Block

```yaml
env:
  - name: REDIS_HOST
    value: redis-cluster-ip-service
  - name: REDIS_PORT
    value: "6379"
  - name: PGUSER
    value: postgres
  - name: PGHOST
    value: postgres-cluster-ip-service
  - name: PGPORT
    value: "5432"
  - name: PGDATABASE
    value: postgres
```

## What Matters Most

- worker gets Redis only
- server gets Redis plus Postgres
- service names are used as connection targets
- `PGPASSWORD` is intentionally missing for now

## Retrieval Cues

- "`env` is an array"
- "worker = Redis only"
- "server = Redis + Postgres"
- "password comes later via secret"
