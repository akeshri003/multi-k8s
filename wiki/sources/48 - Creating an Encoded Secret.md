---
title: Creating an Encoded Secret
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/48 - Creating an Encoded Secret.md
source_date: 2026-04-20
tags:
  - kubernetes
  - secret
  - postgres
  - environmentvariables
  - security
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/47 - Adding Environment Variables to Config.md
  - wiki/sources/46 -Defining Environment Variables.md
---

# Creating an Encoded Secret

## Metadata

- Source type: transcript with diagrams and imperative command walkthrough
- Transcript quality: strong enough to preserve what a Kubernetes `Secret` is for, why the password is handled imperatively here, and the exact secret-creation command pattern
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-20 at 11.09.32 AM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 11.12.22 AM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 11.27.47 AM.png]]
- Derived notes:
  - `derived/notes/48 - Creating an Encoded Secret - Cheat Sheet.md`
  - `derived/notes/48 - Creating an Encoded Secret - Explainer.md`

## Summary

This source introduces `Secret` as a new Kubernetes object type for storing sensitive values such as database passwords, API keys, or SSH keys. Instead of writing `PGPASSWORD` directly into a deployment manifest, the lesson creates a generic secret with an imperative `kubectl create secret` command. The course treats this as the practical exception to the otherwise declarative workflow, because the sensitive value must be supplied at creation time rather than committed into a normal config file.

## Key Claims

- `PGPASSWORD` is a Postgres password, not a Docker or Kubernetes password.
- Sensitive values should not be written into deployment config in plain text.
- Kubernetes `Secret` objects are intended for storing protected values such as passwords, API keys, and SSH keys.
- A `Secret` is another Kubernetes object type, similar in principle to pods, services, or deployments.
- The course creates this secret with an imperative command instead of a config file.
- The reason for the imperative command is to avoid writing the secret value into a normal committed config file.
- The secret type used here is `generic`, which stores arbitrary key-value data.
- The name of the secret matters because deployments will need to refer back to it later.
- Because this secret is created imperatively, it will need to be created manually in any other environment such as production.

## Important Evidence Or Examples

- The lesson introduces `Secret` as an object for values like:
  - database passwords
  - API keys
  - SSH keys
- The command structure shown in the lesson is:

```bash
kubectl create secret generic <secret_name> --from-literal=<key>=<value>
```

- The concrete example shown on screen is:

```bash
kubectl create secret generic pgpassword --from-literal PGPASSWORD=postgres
```

- The screenshots break the command down into parts:
  - `kubectl create` -> imperative object creation
  - `secret generic` -> create a generic secret
  - `pgpassword` -> name of the secret
  - `--from-literal` -> provide the secret data directly in the command
  - `PGPASSWORD=postgres` -> the key/value being stored
- The transcript explicitly contrasts secrets with ordinary values like `REDIS_HOST`, which are not treated as secret because they are already visible in normal service config.

## Important Commands

```bash
kubectl create secret generic pgpassword --from-literal PGPASSWORD=postgres
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson explains how to create the secret, but it still does not show how the server deployment will consume the secret value inside its `env` block.
- The command shown on screen uses a simple demo password, which is fine for the course but not a production practice.
- The lesson treats imperative secret creation as a practical exception, which introduces an operational step that now has to be repeated in every environment.
