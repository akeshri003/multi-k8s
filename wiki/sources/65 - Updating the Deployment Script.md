---
title: Updating the Deployment Script
type: source
status: active
created: 2026-04-22
updated: 2026-04-22
source_path: raw/transcripts/65 - Updating the Deployment Script.md
source_date: 2026-04-22
tags:
  - kubernetes
  - ci
  - cd
  - travis
  - deploy-script
  - git-sha
  - gcloud
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/60 - CI Setup.md
  - wiki/sources/63 - Unique Deployment Images.md
  - wiki/sources/64 - Unique Tags for Built Image.md
  - wiki/sources/66 - Configuring the gcloud CLI on the cloud console.md
  - wiki/analysis/github-actions-gke-deployment-flow.md
---

# Updating the Deployment Script

## Metadata

- Source type: transcript with screenshot embeds whose image files are not currently canonicalized in `raw/assets`
- Transcript quality: strong enough to preserve the actual `.travis.yml` environment variables and the final `deploy.sh` image-tag updates
- Embedded visuals:
  - The transcript references four screenshot files, but no matching local assets were found in `raw/assets`
- Derived notes:
  - `derived/notes/65 - Updating the Deployment Script - Cheat Sheet.md`
  - `derived/notes/65 - Updating the Deployment Script - Explainer.md`

## Summary

This source turns the previous Git-SHA idea into actual CI and deploy-script changes. The course adds `SHA=$(git rev-parse HEAD)` and `CLOUDSDK_CORE_DISABLE_PROMPTS=1` to `.travis.yml`, then updates `deploy.sh` so each image is built with both `latest` and `$SHA`, each of those tags is pushed to Docker Hub, and each Kubernetes deployment is updated to the SHA-specific image tag. The durable lesson is that the pipeline now separates two roles cleanly: `latest` stays as the default image reference for clean applies, while `$SHA` is the immutable version used for deterministic production rollouts.

## Key Claims

- `.travis.yml` should export the current commit SHA into an environment variable called `SHA`.
- `.travis.yml` should also set `CLOUDSDK_CORE_DISABLE_PROMPTS=1` so Google Cloud CLI commands do not block on interactive prompts.
- Each `docker build` command should apply two tags:
  - `:latest`
  - `:$SHA`
- `docker push` must be run once per tag, not once per image name.
- The `kubectl set image` step should use the SHA-tagged image for:
  - `server-deployment`
  - `client-deployment`
  - `worker-deployment`

## Important Evidence Or Examples

The transcript's Travis env block is described as:

```yaml
env:
  global:
    - SHA=$(git rev-parse HEAD)
    - CLOUDSDK_CORE_DISABLE_PROMPTS=1
```

The client build pattern becomes:

```bash
docker build -t DOCKER_ID/multi-client:latest -t DOCKER_ID/multi-client:$SHA -f ./client/Dockerfile ./client
```

The same pattern is then repeated for `multi-server` and `multi-worker`.

The push step doubles as well:

```bash
docker push DOCKER_ID/multi-client:latest
docker push DOCKER_ID/multi-client:$SHA
```

And the deployment update step becomes explicit about SHA-tagged images:

```bash
kubectl set image deployment/client-deployment client=DOCKER_ID/multi-client:$SHA
kubectl set image deployment/server-deployment server=DOCKER_ID/multi-server:$SHA
kubectl set image deployment/worker-deployment worker=DOCKER_ID/multi-worker:$SHA
```

## Important Commands

```bash
git rev-parse HEAD
docker build -t DOCKER_ID/multi-client:latest -t DOCKER_ID/multi-client:$SHA -f ./client/Dockerfile ./client
docker build -t DOCKER_ID/multi-server:latest -t DOCKER_ID/multi-server:$SHA -f ./server/Dockerfile ./server
docker build -t DOCKER_ID/multi-worker:latest -t DOCKER_ID/multi-worker:$SHA -f ./worker/Dockerfile ./worker

docker push DOCKER_ID/multi-client:latest
docker push DOCKER_ID/multi-client:$SHA
docker push DOCKER_ID/multi-server:latest
docker push DOCKER_ID/multi-server:$SHA
docker push DOCKER_ID/multi-worker:latest
docker push DOCKER_ID/multi-worker:$SHA

kubectl set image deployment/client-deployment client=DOCKER_ID/multi-client:$SHA
kubectl set image deployment/server-deployment server=DOCKER_ID/multi-server:$SHA
kubectl set image deployment/worker-deployment worker=DOCKER_ID/multi-worker:$SHA
```

## Modern Equivalent

For this vault, the modern equivalent is to use `GITHUB_SHA` inside GitHub Actions rather than exporting `SHA` from a Travis `env.global` block.

That maps directly onto the durable pattern already captured in [[github-actions-gke-deployment-flow]].

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The pipeline is now operationally deterministic, but it is still partly imperative because it depends on `kubectl set image` after `kubectl apply -f k8s`.
- The new script also assumes the remote cluster already has the required Kubernetes secret and ingress controller setup, which the next lessons move into manual cloud-side steps.
- The screenshot evidence referenced in the transcript is not yet stored canonically in `raw/assets`.
