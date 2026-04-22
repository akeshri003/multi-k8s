---
title: Skaffold Config
type: source
status: active
created: 2026-04-23
updated: 2026-04-23
source_path: raw/transcripts/77 - Skaffold Config.md
source_date: 2026-04-22
tags:
  - kubernetes
  - skaffold
  - local-development
  - ingress
  - config
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/75 - Ingress for Config.md
  - wiki/sources/76 - skaffold overview.md
  - wiki/sources/78 - Live sync changes.md
  - wiki/analysis/skaffold-local-kubernetes-development.md
---

# Skaffold Config

## Metadata

- Source type: update note with one screenshot and a full modernized config example
- Transcript quality: the update note is the primary operational artifact and should be treated as the source of truth
- Embedded visuals:
  - ![[raw/assets/Pasted image 20260423011457.png]]
- Derived notes:
  - `derived/notes/77 - Skaffold Config - Cheat Sheet.md`
  - `derived/notes/77 - Skaffold Config - Explainer.md`

## Summary

This source modernizes the Skaffold setup and, more importantly, separates development-only Kubernetes resources from production HTTPS resources. The update note says to create `k8s-dev/` and `k8s-prod/`, move the certificate, issuer, and production ingress files into `k8s-prod/`, create a development-only ingress in `k8s-dev/`, and configure `skaffold.yaml` to deploy both the base `k8s/` manifests and the development-only overlays. It also updates the Skaffold API version, sync globs, and development Dockerfile environment variables.

## Key Claims

- Production HTTPS resources should be separated from local-development manifests.
- The repo should gain `k8s-dev/` and `k8s-prod/` directories beside the existing `k8s/` directory.
- `certificate.yaml`, `issuer.yaml`, and the production `ingress-service.yaml` belong in `k8s-prod/`.
- Local development should use a separate development ingress manifest in `k8s-dev/`.
- Skaffold should deploy both `./k8s/*` and `./k8s-dev/*`.
- The modern config shown uses `apiVersion: skaffold/v2beta12`.
- Client sync globs should watch JS, CSS, and HTML under `src/**`.
- Server and worker sync globs should watch top-level `*.js`.
- Development images should be distinct from production images.
- `client/Dockerfile.dev` should set `CI=true` and `WDS_SOCKET_PORT=0`.

## Important Config

The source's important preserved snippets are:

```yaml
deploy:
  kubectl:
    manifests:
      - ./k8s/*
      - ./k8s-dev/*
```

```yaml
apiVersion: skaffold/v2beta12
```

```dockerfile
FROM node:16-alpine
ENV CI=true
ENV WDS_SOCKET_PORT=0
```

## Important Evidence Or Examples

- The update note explicitly warns that the user likely just finished HTTPS work and therefore needs to avoid conflicts between production TLS resources and local dev resources.
- The full example config uses separate development image names such as `client-skaffold`, `worker-skaffold`, and `server-skaffold`.
- The source ends by saying to run `skaffold dev`.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This source is more of a modernization patch than a clean original lesson transcript, so the update note should dominate interpretation.
- The exact Skaffold API version may age again in the future.
