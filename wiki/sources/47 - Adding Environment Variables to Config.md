---
title: Adding Environment Variables to Config
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/47 - Adding Environment Variables to Config.md
source_date: 2026-04-20
tags:
  - kubernetes
  - environmentvariables
  - deployment
  - redis
  - postgres
  - server
  - worker
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/46 -Defining Environment Variables.md
  - wiki/sources/30 - Express API deployment config.md
  - wiki/sources/33 - The Worker Deployment.md
---

# Adding Environment Variables to Config

## Metadata

- Source type: transcript with embedded YAML screenshots
- Transcript quality: strong enough to preserve the actual `env` blocks added to `worker-deployment` and `server-deployment`
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-20 at 11.04.40 AM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 11.07.32 AM.png]]
- Derived notes:
  - `derived/notes/47 - Adding Environment Variables to Config - Cheat Sheet.md`
  - `derived/notes/47 - Adding Environment Variables to Config - Explainer.md`

## Summary

This source turns the earlier environment-variable overview into actual deployment config. The worker gets the minimum Redis connection settings it needs, while the server gets a larger set of Redis and Postgres values. The lesson explicitly leaves out `PGPASSWORD` for now and says that value will be handled differently in the next section instead of being written directly into the deployment manifest.

## Key Claims

- Environment variables are added under the container's `env` field as an array of `name` and `value` entries.
- The worker only needs Redis connection information in this step.
- The worker uses `REDIS_HOST` and `REDIS_PORT`.
- `REDIS_HOST` should be the name of the Redis `ClusterIP` service.
- `REDIS_PORT` stays at the default Redis port `6379`.
- The server needs both Redis and Postgres connection information.
- The server gets `REDIS_HOST`, `REDIS_PORT`, `PGUSER`, `PGHOST`, `PGPORT`, and `PGDATABASE`.
- `PGHOST` should be the name of the Postgres `ClusterIP` service.
- `PGPORT` stays at the default Postgres port `5432`.
- The course intentionally leaves out `PGPASSWORD` in this lesson so it can be handled more securely in the next one.

## Important Evidence Or Examples

- The worker deployment is updated with:

```yaml
env:
  - name: REDIS_HOST
    value: redis-cluster-ip-service
  - name: REDIS_PORT
    value: "6379"
```

- The server deployment is updated with:

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

- The screenshots make the two deployment updates concrete:
  - worker gets only the Redis variables
  - server gets Redis plus Postgres variables
- The source explicitly says `PGPASSWORD` is being deferred because it should not be written into the config as plain text.

## Important Commands

- No shell command is central in this source.
- The lesson is focused on updating deployment YAML.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson wires in most runtime values, but it still leaves out `PGPASSWORD`, so the server is not fully configured yet.
- The source uses plain `value` entries for the ordinary settings, but it still has not shown how Kubernetes will pull a value from a `Secret`.
- The screenshots preserve the exact deployment fragments, but the actual apply-and-verify step still belongs to a later lesson.
