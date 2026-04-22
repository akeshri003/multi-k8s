---
title: Live sync changes
type: source
status: active
created: 2026-04-23
updated: 2026-04-23
source_path: raw/transcripts/78 - Live sync changes.md
source_date: 2026-04-22
tags:
  - kubernetes
  - skaffold
  - local-development
  - sync
  - react
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/76 - skaffold overview.md
  - wiki/sources/77 - Skaffold Config.md
  - wiki/analysis/skaffold-local-kubernetes-development.md
---

# Live sync changes

## Metadata

- Source type: transcript walkthrough showing the first practical `skaffold dev` loop
- Transcript quality: strong enough to preserve the deploy section intent, the sync fallback behavior, and the first end-to-end live-edit demo
- Embedded visuals:
  - No local screenshots are embedded in this source
- Derived notes:
  - `derived/notes/78 - Live sync changes - Cheat Sheet.md`
  - `derived/notes/78 - Live sync changes - Explainer.md`

## Summary

This source finishes the first Skaffold setup and demonstrates the live-edit loop. Skaffold watches the client project, applies the selected Kubernetes manifests, and syncs JS, CSS, or HTML changes directly into the running client containers. If a non-synced file changes, Skaffold falls back to rebuilding the image. The demo then shows a React text change appearing in the browser after `skaffold dev` reports a file sync.

## Key Claims

- The `deploy.kubectl.manifests` section tells Skaffold which Kubernetes YAML files to manage.
- Starting Skaffold with `skaffold dev` both deploys and begins file watching.
- For synced file types, Skaffold injects changed files into the running container instead of forcing a full rebuild.
- For non-synced file types, Skaffold falls back to rebuilding the image and updating the cluster.
- The Create React App dev server inside the container recompiles when the synced source file changes.
- Multiple log lines appear because the client deployment still runs multiple replicas.

## Important Commands

```bash
skaffold dev
```

## Important Evidence Or Examples

- The transcript uses `k8s/client-deployment.yaml` as the initial managed manifest example.
- The demo edits `client/src/App.js` and changes the heading text.
- Skaffold logs a file sync event, and then the client containers log successful recompilation.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson only shows the client path cleanly; the update note in the prior source is still needed for the fuller multi-service dev setup.
- The transcript notes that browser auto-refresh can be finicky even when the file sync succeeds.
