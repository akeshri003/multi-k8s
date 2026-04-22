---
title: Rebuilding the Client Image
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/21 - Rebuilding the Client Image.md
source_date: 2026-04-19
tags:
  - kubernetes
  - docker
  - image-build
  - docker-hub
  - multi-client
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/20 - Updating Deployment Images.md
  - wiki/sources/22 - Triggering Deployment Updates.md
---

# Rebuilding the Client Image

## Metadata

- Source type: transcript with code-edit and Docker command workflow
- Transcript quality: strong enough to preserve the concrete source-code change, image rebuild, and Docker Hub push sequence
- Embedded visuals:
  - None.
- Derived notes:
  - `derived/notes/21 - Rebuilding the Client Image - Cheat Sheet.md`
  - `derived/notes/21 - Rebuilding the Client Image - Explainer.md`

## Summary

This source performs the application-side change that the deployment will later need to pick up. It goes back to the `complex` project, edits the React client source, changes the page title from `Fib calculator` to `Fib calculator version two`, rebuilds the `multi-client` Docker image, and pushes that image to Docker Hub. The result is a newer image in the registry, but the deployment has not yet been told how to use that new version.

## Key Claims

- The simplest visible way to prove an image changed is to edit user-facing text in the app.
- The chosen code change is to modify the `h1` text in `App.js`.
- After a source-code change, the image must be rebuilt before the new version exists.
- After the rebuild, the image must be pushed to Docker Hub so the cluster can potentially pull it.
- Even after Docker Hub has the newest image, the deployment still needs an explicit mechanism to move its pods onto that new image version.

## Important Evidence Or Examples

- The transcript edits the React client so the page title changes from:

```text
Fib calculator
```

to:

```text
Fib calculator version two
```

- The rebuild step is described as:

```bash
docker build -t <docker-id>/multi-client .
```

- The push step is:

```bash
docker push <docker-id>/multi-client
```

- The source is explicit that the push means Docker Hub now has the latest `multi-client` image, but the Kubernetes deployment has not yet been updated to use it.

## Important Commands

```bash
docker build -t <docker-id>/multi-client .
docker push <docker-id>/multi-client
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript intentionally uses the default unversioned image name here, which sets up the next lesson's question of how Kubernetes is supposed to notice that the image contents changed.
- The source shows how to create a new image version in Docker Hub, but it still does not show how to connect that registry change back to the deployment state in the cluster.
