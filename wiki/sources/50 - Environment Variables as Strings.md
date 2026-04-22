---
title: Environment Variables as Strings
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/50 - Environment Variables as Strings.md
source_date: 2026-04-20
tags:
  - kubernetes
  - environmentvariables
  - yaml
  - deployment
  - debugging
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/47 - Adding Environment Variables to Config.md
  - wiki/sources/49 - Postgres Environment Variable Fix.md
---

# Environment Variables as Strings

## Metadata

- Source type: screenshot-driven fix lesson
- Transcript quality: partial but strong enough to preserve the exact YAML correction and the successful re-apply result
- Embedded visuals:
  - ![[Screenshot 2026-04-20 at 11.41.03 AM.png]]
  - ![[Screenshot 2026-04-20 at 11.41.14 AM.png]]
  - ![[Screenshot 2026-04-20 at 11.41.39 AM.png]]
- Derived notes:
  - `derived/notes/50 - Environment Variables as Strings - Cheat Sheet.md`
  - `derived/notes/50 - Environment Variables as Strings - Explainer.md`

## Summary

This source resolves the apply error introduced in the previous lesson. The problem was that numeric-looking environment-variable values such as Redis and Postgres ports were written as numbers, but Kubernetes expects environment-variable `value` fields to be strings. The fix is simple: quote those values in the deployment YAML. After that change, `kubectl apply -f k8s` succeeds and both deployments are configured correctly.

## Key Claims

- Environment-variable `value` fields must be strings in these deployment manifests.
- Writing port values like `6379` or `5432` as bare numbers leads to the earlier `cannot convert int64 to string` error.
- Quoting those port values fixes the YAML typing problem.
- The worker deployment needs `REDIS_PORT` written as a string.
- The server deployment needs both `REDIS_PORT` and `PGPORT` written as strings.
- After those fixes, `kubectl apply -f k8s` completes successfully for the updated deployments.

## Important Evidence Or Examples

- The worker deployment fix shown in the screenshot is:

```yaml
- name: REDIS_PORT
  value: "6379"
```

- The server deployment fixes shown in the screenshot are:

```yaml
- name: REDIS_PORT
  value: "6379"
- name: PGPORT
  value: "5432"
```

- The terminal screenshots show the before-and-after flow:

```bash
kubectl apply -f k8s
```

- The first apply fails with:

```text
cannot convert int64 to string
```

- The second apply succeeds, with output indicating:
  - `deployment.apps/server-deployment configured`
  - `deployment.apps/worker-deployment configured`

## Important Commands

```bash
kubectl apply -f k8s
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson fixes the YAML typing issue, but it still does not yet prove the app can end-to-end read from and write to Redis and Postgres successfully.
- The screenshots for this lesson currently live at the vault root rather than under `raw/assets`, so the evidence is usable but not yet in the canonical raw-assets location.
- The deployments now apply cleanly, but the next meaningful check is runtime behavior rather than config syntax.
