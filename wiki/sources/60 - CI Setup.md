---
title: CI Setup
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/60 - CI Setup.md
source_date: 2026-04-20
tags:
  - kubernetes
  - ci
  - cd
  - travis
  - github-actions
  - gke
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/59 - The Deployment Process.md
  - wiki/sources/48 - Creating an Encoded Secret.md
  - wiki/analysis/github-actions-gke-deployment-flow.md
---

# CI Setup

## Metadata

- Source type: transcript with one local flow-diagram screenshot
- Transcript quality: strong enough to preserve the full Travis-based deployment pipeline and the reason image updates still need an imperative deployment step
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-20 at 4.42.10 PM.png]]
- Derived notes:
  - `derived/notes/60 - CI Setup - Cheat Sheet.md`
  - `derived/notes/60 - CI Setup - Explainer.md`

## Summary

This source explains the full CI/CD pipeline the course wants to build around the Kubernetes app. In the course's historical setup, Travis installs the Google Cloud SDK, authenticates to Google Cloud, logs into Docker, builds a test image, runs tests, builds and pushes all production images, applies the `k8s/` configs, and then imperatively updates deployments so they use the newly pushed image versions. The main practical takeaway is not "use Travis" so much as "CI must build, test, authenticate, push, apply, and then force deployments onto the correct image tags."

## Key Claims

- The course's `travis.yml` is more complex than earlier course CI files because it does both testing and deployment.
- The CI environment must install the Google Cloud SDK so it can talk to the Kubernetes cluster.
- The CI environment must authenticate to Google Cloud before it can apply cluster config.
- The CI environment must log into Docker before it can push rebuilt images.
- A test image for `multi-client` is built and used to run tests.
- If tests pass, a deployment script builds and pushes all three images:
  - `multi-client`
  - `multi-server`
  - `multi-worker`
- After pushing images, CI applies every config file in `k8s/`.
- Reapplying manifests alone is not enough to make deployments use a newly built image, so the pipeline also runs imperative image-update commands.

## Important Evidence Or Examples

- The main flow diagram shows the whole Travis config sequence:
  - install Google Cloud SDK CLI
  - configure Google Cloud auth
  - log into Docker CLI
  - build the test image
  - run tests
  - run a deployment script if tests pass
  - build and push all images
  - apply all configs in the `k8s` folder
  - imperatively set latest images on each deployment
- The transcript explains why the final imperative image-update step exists: earlier lessons already showed that reapplying a deployment config does not automatically detect new image content behind the same image reference.
- The transcript explicitly says the goal is to keep the Kubernetes cluster in sync with the repo's `k8s/` directory.

## Important Commands

- The transcript does not paste the final CI file yet.
- The operational steps it describes include:
  - Docker login
  - build test image
  - run tests
  - build and push the production images
  - `kubectl apply -f k8s`
  - imperative deployment image updates

## Modern Equivalent

The course uses Travis CI, but for this vault the modern equivalent is GitHub Actions.

A practical GitHub Actions based flow is:

1. check out the repo
2. authenticate to Google Cloud
3. get GKE credentials for `kubectl`
4. log in to Docker Hub
5. build and run tests
6. build and push versioned images
7. apply the `k8s/` manifests
8. update deployments to the exact pushed image tags

The durable GitHub Actions version of this now lives in [[github-actions-gke-deployment-flow]].

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The CI logic is still historically tied to Travis CI even though the underlying deployment ideas map cleanly onto GitHub Actions.
- The source describes the flow clearly, but it still stops before showing the actual CI file contents.
- The image-update workaround remains operationally awkward: declarative manifests describe the app, but the pipeline still needs an imperative step unless the image-tag strategy is made fully declarative.
