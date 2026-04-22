---
title: Skaffold Local Kubernetes Development
type: analysis
status: active
created: 2026-04-23
updated: 2026-04-23
tags:
  - kubernetes
  - skaffold
  - local-development
  - live-reload
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/76 - skaffold overview.md
  - wiki/sources/77 - Skaffold Config.md
  - wiki/sources/78 - Live sync changes.md
---

# Skaffold Local Kubernetes Development

## Question

What problem is Skaffold solving in this Kubernetes sequence, and what is the recommended local-development setup after the HTTPS production resources were introduced?

## Short Answer

Skaffold fixes the slow local inner loop.

Without it, code changes usually mean:

1. rebuild image
2. reapply manifests
3. wait for pods to update

With Skaffold, the loop becomes:

- watch local files
- either sync them directly into running containers
- or fall back to rebuild/redeploy when sync is not possible

## The Two Modes

### Mode 1: Rebuild And Redeploy

This is slower but general.

If the changed file is outside the configured sync patterns, Skaffold rebuilds the image and updates the cluster.

### Mode 2: File Sync

This is the fast path.

Skaffold copies changed files into the running container, then the app's own dev tooling reacts.

That works well here because:

- the React client already hot-reloads
- the Node services can use file-watching tooling such as Nodemon

## Why The Repo Structure Changes

Once production HTTPS resources exist, local development should stop deploying those production-only files.

The modernized structure from the source set is:

- `k8s/` for shared base manifests
- `k8s-dev/` for development-only ingress
- `k8s-prod/` for certificate, issuer, and production ingress

This keeps local development from colliding with domain-specific production TLS resources.

## Durable Skaffold Shape

The key preserved deployment shape is:

```yaml
deploy:
  kubectl:
    manifests:
      - ./k8s/*
      - ./k8s-dev/*
```

And the main loop starts with:

```bash
skaffold dev
```

## What The Demo Proves

The final lesson in this batch shows the fast path:

1. run `skaffold dev`
2. edit `client/src/App.js`
3. Skaffold syncs the changed file
4. the React app recompiles inside the container
5. the browser shows the update

## Common Failure Boundaries

- production ingress/certificate files accidentally mixed into dev deploys
- sync globs too narrow, causing unnecessary rebuilds
- dev images and production images not kept distinct
- browser refresh behavior appears inconsistent even though the file sync worked

## Durable Takeaway

Skaffold is the missing development ergonomics layer for this repo's Kubernetes setup.

The key idea is not just "run another CLI."

It is:

- separate dev from prod manifests
- define which files can live-sync
- use `skaffold dev` to restore a fast feedback loop
