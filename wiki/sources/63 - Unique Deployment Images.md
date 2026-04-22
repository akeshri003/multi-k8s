---
title: Unique Deployment Images
type: source
status: active
created: 2026-04-22
updated: 2026-04-22
source_path: raw/transcripts/63 - Unique Deployment Images.md
source_date: 2026-04-22
tags:
  - kubernetes
  - ci
  - cd
  - deploy-script
  - docker
  - image-tags
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/60 - CI Setup.md
  - wiki/sources/64 - Unique Tags for Built Image.md
  - wiki/sources/65 - Updating the Deployment Script.md
---

# Unique Deployment Images

## Metadata

- Source type: transcript with screenshot embeds whose image files are not currently canonicalized in `raw/assets`
- Transcript quality: strong enough to preserve the first draft of `deploy.sh` and the reason it is still incomplete
- Embedded visuals:
  - The transcript references six screenshot files from April 22, 2026, but no matching local assets were found in `raw/assets`
- Derived notes:
  - `derived/notes/63 - Unique Deployment Images - Cheat Sheet.md`
  - `derived/notes/63 - Unique Deployment Images - Explainer.md`

## Summary

This source starts the real deployment script for the production pipeline. It writes the first pass of `deploy.sh`: build the `client`, `server`, and `worker` images, push them to Docker Hub, apply every manifest in `k8s/`, and then run `kubectl set image` against each deployment. The key lesson is that this seemingly reasonable script is still wrong because it is only using the implicit `latest` image reference, which earlier lessons already showed is not enough to force a deployment rollout.

## Key Claims

- The deployment script should automate the same repeated production sequence:
  - build images
  - push images
  - apply Kubernetes config
  - imperatively update deployment images
- The script builds three production images:
  - `multi-client`
  - `multi-server`
  - `multi-worker`
- The course assumes Docker login already happened earlier in `.travis.yml`, so `docker push` can run directly inside the deploy script.
- The course also assumes Google Cloud and `kubectl` were already configured earlier in CI, so `kubectl apply -f k8s` can run directly.
- A naive `kubectl set image` command that still points at the same implicit `latest` image will not trigger the deployment update the pipeline actually needs.

## Important Evidence Or Examples

The first-pass build commands are described as:

```bash
docker build -t DOCKER_ID/multi-client -f ./client/Dockerfile ./client
docker build -t DOCKER_ID/multi-server -f ./server/Dockerfile ./server
docker build -t DOCKER_ID/multi-worker -f ./worker/Dockerfile ./worker
```

The first-pass push and apply steps are:

```bash
docker push DOCKER_ID/multi-client
docker push DOCKER_ID/multi-server
docker push DOCKER_ID/multi-worker
kubectl apply -f k8s
```

The transcript then walks into the real problem by drafting `kubectl set image` commands and reminding the reader that this still points deployments at the same mutable image reference.

## Important Commands

```bash
docker build -t DOCKER_ID/multi-client -f ./client/Dockerfile ./client
docker build -t DOCKER_ID/multi-server -f ./server/Dockerfile ./server
docker build -t DOCKER_ID/multi-worker -f ./worker/Dockerfile ./worker

docker push DOCKER_ID/multi-client
docker push DOCKER_ID/multi-server
docker push DOCKER_ID/multi-worker

kubectl apply -f k8s
kubectl set image deployment/server-deployment server=DOCKER_ID/multi-server
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source intentionally leaves the deployment script half-correct: it automates the right stages, but it still has the old image-tagging flaw.
- The transcript references multiple screenshots, but those assets are not yet stored canonically in `raw/assets`.
- The next lesson is required to explain how the script will generate unique image references that make `kubectl set image` actually change something.
