# Environment Variables as Strings Cheat Sheet

## Source

- Transcript: `raw/transcripts/50 - Environment Variables as Strings.md`
- Wiki source page: [[50 - Environment Variables as Strings]]

## Key Visuals

- ![[Screenshot 2026-04-20 at 11.41.03 AM.png]]
- ![[Screenshot 2026-04-20 at 11.41.14 AM.png]]
- ![[Screenshot 2026-04-20 at 11.41.39 AM.png]]

## Core Idea

The env `value` fields must be strings.

Bare numbers caused the apply error.

## Fixes

```yaml
- name: REDIS_PORT
  value: "6379"
```

```yaml
- name: PGPORT
  value: "5432"
```

## Where The Fix Applies

- worker: quote `REDIS_PORT`
- server: quote `REDIS_PORT` and `PGPORT`

## Result

```bash
kubectl apply -f k8s
```

After quoting the numbers, the apply succeeds.

## Retrieval Cues

- "env values are strings"
- "quote the ports"
- "`cannot convert int64 to string`"
