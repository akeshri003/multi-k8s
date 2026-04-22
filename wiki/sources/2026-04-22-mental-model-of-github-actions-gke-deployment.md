---
title: Mental model of deployment to Google Kubernetes Engine using GitHub Actions
type: source
status: active
created: 2026-04-22
updated: 2026-04-22
source_path: raw/notes/Mental model of deployment to Google Kubernetes engine using GitHub actions.md
source_date: 2026-04-22
tags:
  - github-actions
  - kubernetes
  - gke
  - ci
  - cd
  - runner
  - kubectl
related:
  - wiki/topics/kubernetes.md
  - wiki/analysis/github-actions-gke-deployment-flow.md
  - wiki/analysis/multi-k8s-github-actions-files-and-flow.md
  - wiki/sources/65 - Updating the Deployment Script.md
---

# Mental model of deployment to Google Kubernetes Engine using GitHub Actions

## Metadata

- Source type: raw explanatory note capturing a question and answer from the recent GitHub Actions and GKE setup discussion
- Note quality: strong enough to preserve the runner-versus-cluster mental model and the step-by-step boundary between GitHub Actions and GKE
- Embedded visuals:
  - None

## Summary

This note corrects a common mental-model error in the GitHub Actions deployment flow. The workflow does run on a temporary Ubuntu VM, but that VM is a GitHub-hosted runner, not a Google Compute Engine instance. The runner checks out the repository, obtains short-lived credentials, and uses `kubectl` as a client to send API calls to the GKE control plane. The actual Kubernetes work such as scheduling pods, pulling images, and starting containers happens inside GKE after the API server accepts those requests.

## Key Claims

- `runs-on: ubuntu-latest` means the job runs on a fresh GitHub-hosted Ubuntu runner, not on a VM inside GCP.
- The runner is ephemeral and is destroyed when the workflow job ends.
- `actions/checkout` clones the repository onto the runner's local disk for that one workflow run.
- `google-github-actions/auth` gives the runner short-lived Google Cloud credentials.
- `google-github-actions/get-gke-credentials` writes cluster access config on the runner so `kubectl` can talk to the GKE API server.
- `kubectl apply -f k8s` does not do Kubernetes work locally on the runner; it sends API requests to the GKE control plane.
- Pod scheduling, image pulls, and container startup happen inside GKE after those API calls arrive.

## Important Evidence Or Examples

The note rewrites the deployment path into distinct execution locations:

- GitHub infrastructure:
  - create the temporary Ubuntu runner
  - clone the repo
  - acquire credentials
  - run `kubectl`
- Google Cloud / GKE:
  - receive the API requests
  - reconcile desired state
  - schedule pods
  - pull images
  - start containers

It also gives a step-by-step breakdown of the workflow:

1. `runs-on: ubuntu-latest`
2. `actions/checkout@v4`
3. `google-github-actions/auth@v3`
4. `google-github-actions/get-gke-credentials@v3`
5. `kubectl apply -f k8s`
6. `kubectl rollout status`
7. destroy the runner

## Related Entities

- None yet.

## Related Topics

- [[kubernetes]]

## Tensions Or Open Questions

- This note is a conceptual clarification rather than official vendor documentation, so it is best used as the wiki's working mental model rather than as a primary citation about GitHub or GKE internals.
- The note clarifies where commands run, but it still does not specify the final deploy workflow file this repo should standardize on once image-build and secret steps are fully automated.
