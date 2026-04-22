# Skaffold Config Cheat Sheet

## Source

- Transcript: `raw/transcripts/77 - Skaffold Config.md`
- Wiki source page: [[77 - Skaffold Config]]

## Key Visual

- ![[raw/assets/Pasted image 20260423011457.png]]

## Repo Split

- `k8s/` = shared base manifests
- `k8s-dev/` = development-only ingress
- `k8s-prod/` = certificate, issuer, production ingress

## Skaffold Deploy Section

```yaml
deploy:
  kubectl:
    manifests:
      - ./k8s/*
      - ./k8s-dev/*
```

## Key Updates

- `apiVersion: skaffold/v2beta12`
- use dev-only image names
- add sync globs
- add `CI=true`
- add `WDS_SOCKET_PORT=0`

## Run

```bash
skaffold dev
```
