---
title: Unique Tags for Built Image
type: source
status: active
created: 2026-04-22
updated: 2026-04-22
source_path: raw/transcripts/64 - Unique Tags for Built Image.md
source_date: 2026-04-22
tags:
  - kubernetes
  - ci
  - cd
  - git
  - image-tags
  - docker
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/63 - Unique Deployment Images.md
  - wiki/sources/65 - Updating the Deployment Script.md
  - wiki/analysis/github-actions-gke-deployment-flow.md
---

# Unique Tags for Built Image

## Metadata

- Source type: transcript with screenshot embeds whose image files are not currently canonicalized in `raw/assets`
- Transcript quality: strong enough to preserve the actual reason `latest` is insufficient and the Git-SHA workaround the course chooses
- Embedded visuals:
  - The transcript references three screenshot files from April 22, 2026, but no matching local assets were found in `raw/assets`
- Derived notes:
  - `derived/notes/64 - Unique Tags for Built Image - Cheat Sheet.md`
  - `derived/notes/64 - Unique Tags for Built Image - Explainer.md`

## Summary

This source fixes the conceptual problem from the previous lesson. The deployment pipeline needs a unique image tag on every build, otherwise Kubernetes sees no image change and skips the rollout. The course chooses the current Git commit SHA as that unique tag, while still also maintaining `latest` so a fresh cluster or a new engineer can apply the manifests and automatically get the newest default images.

## Key Claims

- A deployment update must point at an image tag that is different from the one currently running.
- The course does not want to hand-maintain version numbers such as `v2`, `v3`, and `v4`.
- `git rev-parse HEAD` returns the current commit SHA, which can be used as a unique image version identifier.
- The build step should apply two tags to each image:
  - `latest`
  - the current Git SHA
- Using the Git SHA makes debugging easier because the running image version maps directly back to an exact Git commit.
- Keeping `latest` still matters because the deployment manifests themselves rely on the implicit latest-tag behavior when someone applies the repo from scratch.

## Important Evidence Or Examples

The transcript explicitly proposes a two-tag build pattern:

```text
DOCKER_ID/multi-client:latest
DOCKER_ID/multi-client:$SHA
```

It then explains the debugging benefit:

- inspect which image tag the cluster is running
- read the Git SHA from that tag
- `git checkout` that commit locally
- debug the exact same code version that is running in production

It also explains why `latest` is kept:

- a new clone of the project can still `kubectl apply -f k8s`
- the deployment manifests will implicitly pull the newest default image

## Important Commands

```bash
git rev-parse HEAD
git log
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The Git-SHA strategy solves deterministic image identity, but the course is still using an imperative `kubectl set image` step rather than a fully declarative image-update workflow.
- The transcript references screenshots that are not yet preserved in `raw/assets`.
- The next lesson still has to show the exact `.travis.yml` and `deploy.sh` edits that turn this tag strategy into a working script.
