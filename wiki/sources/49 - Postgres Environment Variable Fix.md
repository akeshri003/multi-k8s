---
title: Postgres Environment Variable Fix
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/49 - Postgres Environment Variable Fix.md
source_date: 2026-04-20
tags:
  - kubernetes
  - environmentvariables
  - secret
  - postgres
  - server
  - deployment
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/47 - Adding Environment Variables to Config.md
  - wiki/sources/48 - Creating an Encoded Secret.md
---

# Postgres Environment Variable Fix

## Metadata

- Source type: transcript with embedded deployment screenshots and terminal output
- Transcript quality: strong enough to preserve how the `pgpassword` secret is consumed by the server and Postgres deployments, plus the resulting `kubectl apply` type error that sets up the next lesson
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-20 at 11.37.52 AM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 11.38.06 AM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 11.38.34 AM.png]]
- Derived notes:
  - `derived/notes/49 - Postgres Environment Variable Fix - Cheat Sheet.md`
  - `derived/notes/49 - Postgres Environment Variable Fix - Explainer.md`

## Summary

This source wires the previously created `pgpassword` secret into both the server deployment and the Postgres deployment. Instead of using plain `value` fields, both deployments now use `valueFrom.secretKeyRef` to load the password from the `pgpassword` secret. The lesson then applies the config and intentionally hits an error: Kubernetes rejects the patch because some numeric environment-variable values are still being treated as numbers instead of strings. That failure becomes the setup for the next fix.

## Key Claims

- The server deployment should receive the database password from the `pgpassword` secret.
- The Postgres deployment should also receive the password from the same secret so the database uses the same password the server expects.
- The environment variable name inside the container can be independent from the secret name, but it must match what the containerized application expects.
- The server image expects an environment variable named `PGPASSWORD`.
- For secret-backed env vars, `valueFrom.secretKeyRef` is used instead of `value`.
- The secret name referenced here is `pgpassword`.
- The key referenced inside that secret is `PGPASSWORD`.
- Applying the updated YAML produces an error because some env values are still numeric and Kubernetes expects strings in those fields.

## Important Evidence Or Examples

- The server deployment is extended with:

```yaml
- name: PGPASSWORD
  valueFrom:
    secretKeyRef:
      name: pgpassword
      key: PGPASSWORD
```

- The Postgres deployment is extended with:

```yaml
env:
  - name: POSTGRES_PASSWORD
    valueFrom:
      secretKeyRef:
        name: pgpassword
        key: PGPASSWORD
```

- The transcript explicitly explains the distinction between:
  - environment variable name inside the container
  - secret object name
  - key name inside the secret
- The terminal output shows:

```bash
kubectl create secret generic pgpassword --from-literal PGPASSWORD=postgres
kubectl get secrets
kubectl apply -f k8s
```

- The `kubectl apply -f k8s` step fails with:

```text
cannot convert int64 to string
```

- The error appears while patching the deployments, which strongly suggests the issue is with numeric env values such as ports being written as numbers rather than strings in the deployment YAML.

## Important Commands

```bash
kubectl create secret generic pgpassword --from-literal PGPASSWORD=postgres
kubectl get secrets
kubectl apply -f k8s
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source successfully shows how to consume a secret, but it deliberately stops on the apply error before showing the final corrected YAML.
- The lesson implies the problem is type-related in the env values, but the actual fix is deferred to the next source.
- The server and Postgres now both reference the secret, but the worker still does not have any Postgres secret-based configuration in this lesson.
