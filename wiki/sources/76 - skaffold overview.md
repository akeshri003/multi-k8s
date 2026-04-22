---
title: skaffold overview
type: source
status: active
created: 2026-04-23
updated: 2026-04-23
source_path: raw/transcripts/76 - skaffold overview.md
source_date: 2026-04-22
tags:
  - kubernetes
  - skaffold
  - local-development
  - live-reload
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/77 - Skaffold Config.md
  - wiki/sources/78 - Live sync changes.md
  - wiki/analysis/skaffold-local-kubernetes-development.md
---

# skaffold overview

## Metadata

- Source type: conceptual transcript with one overview screenshot
- Transcript quality: strong enough to preserve the two Skaffold update modes and why local Kubernetes development feels awkward without it
- Embedded visuals:
  - ![[raw/assets/Pasted image 20260423010827.png]]
- Derived notes:
  - `derived/notes/76 - skaffold overview - Cheat Sheet.md`
  - `derived/notes/76 - skaffold overview - Explainer.md`

## Summary

This source introduces Skaffold as the local-development bridge for Kubernetes. Without it, every source-code change would require a full image rebuild and cluster re-apply. Skaffold watches local project files and then either rebuilds and redeploys the image or directly syncs changed files into running containers, which works especially well for tools like Create React App and Nodemon that already reload on file changes.

## Key Claims

- Plain Kubernetes local development is awkward compared with Docker Compose volume mounts.
- Without an extra tool, code changes require rebuilding the image and reapplying manifests.
- Skaffold watches local project directories for changes.
- Skaffold has two useful modes:
  - rebuild image and redeploy
  - sync changed files directly into the running container
- Direct file sync works well when the app inside the container already watches files, such as Create React App or Nodemon.

## Important Evidence Or Examples

- The source compares Skaffold's value to the earlier Docker Compose volume-mount workflow.
- The lesson uses the client React app as the motivating example.
- The transcript explicitly says mode two is faster because it avoids a full image rebuild path.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript is introductory and does not yet show the actual `skaffold.yaml` fields needed to enable either mode.
- The lesson is about local development, not production deploy automation.
