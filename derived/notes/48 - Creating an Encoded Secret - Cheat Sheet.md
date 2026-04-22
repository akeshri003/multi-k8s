# Creating an Encoded Secret Cheat Sheet

## Source

- Transcript: `raw/transcripts/48 - Creating an Encoded Secret.md`
- Wiki source page: [[48 - Creating an Encoded Secret]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 11.09.32 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 11.12.22 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 11.27.47 AM.png]]

## Core Idea

Do not put `PGPASSWORD` directly into the deployment YAML.

Store it in a Kubernetes `Secret` instead.

## Main Command

```bash
kubectl create secret generic pgpassword --from-literal PGPASSWORD=postgres
```

## Command Shape

```bash
kubectl create secret generic <secret_name> --from-literal=<key>=<value>
```

## What Matters Most

- `Secret` is a Kubernetes object for sensitive values
- type here is `generic`
- secret name here is `pgpassword`
- key being stored is `PGPASSWORD`
- this must be created manually in each environment

## Retrieval Cues

- "password goes in a secret"
- "`generic` secret"
- "imperative exception to declarative flow"
