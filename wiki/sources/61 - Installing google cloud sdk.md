---
title: Installing google cloud sdk
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/61 - Installing google cloud sdk.md
source_date: 2026-04-20
tags:
  - kubernetes
  - ci
  - cd
  - travis
  - gcloud
  - github-actions
  - gke
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/60 - CI Setup.md
  - wiki/analysis/github-actions-gke-deployment-flow.md
---

# Installing google cloud sdk

## Metadata

- Source type: transcript with one embedded Travis config code block
- Transcript quality: strong enough to preserve the old Travis bootstrap commands and the security model the course used at the time
- Embedded visuals:
  - No local screenshots are embedded in this source
- Derived notes:
  - `derived/notes/61 - Installing google cloud sdk - Cheat Sheet.md`
  - `derived/notes/61 - Installing google cloud sdk - Explainer.md`

## Summary

This source begins the concrete CI file by showing how the course's old Travis pipeline manually installs the Google Cloud SDK, installs `kubectl`, and authenticates with a long-lived Google Cloud service-account JSON file. The deeper durable idea is that CI needs cloud credentials plus cluster tooling before it can deploy. For this vault, the modern replacement is not to `curl`-install the SDK inside every run, but to use official GitHub Actions for Google Cloud auth and GKE credentials.

## Key Claims

- The course puts this logic in `.travis.yml`.
- The Travis environment needs Docker available before the build starts.
- The course manually installs the Google Cloud SDK with `curl https://sdk.cloud.google.com | bash`.
- The shell is then updated with `source $HOME/google-cloud-sdk/path.bash.inc`.
- `gcloud components update kubectl` is used so the CI environment can run `kubectl`.
- The course authenticates with `gcloud auth activate-service-account --key-file service-account.json`.
- The transcript explicitly says the `service-account.json` credentials are highly sensitive.

## Important Evidence Or Examples

The source gives this Travis bootstrap block:

```yaml
sudo: required

services:
  - docker

before_install:
  - curl https://sdk.cloud.google.com | bash > /dev/null;
  - source $HOME/google-cloud-sdk/path.bash.inc
  - gcloud components update kubectl
  - gcloud auth activate-service-account --key-file service-account.json
```

The transcript explains the meaning of that block in stages:

- install Google Cloud SDK
- load the shell config that SDK install created
- install or update `kubectl`
- authenticate to Google Cloud so CI can reach the cluster

## Important Commands

```bash
curl https://sdk.cloud.google.com | bash > /dev/null
source $HOME/google-cloud-sdk/path.bash.inc
gcloud components update kubectl
gcloud auth activate-service-account --key-file service-account.json
```

## Modern Equivalent

For this vault, the modern GitHub Actions equivalent is:

```yaml
- uses: actions/checkout@v4

- uses: google-github-actions/auth@v3
  with:
    project_id: YOUR_PROJECT_ID
    workload_identity_provider: YOUR_WORKLOAD_IDENTITY_PROVIDER
    service_account: YOUR_SERVICE_ACCOUNT

- uses: google-github-actions/get-gke-credentials@v3
  with:
    cluster_name: YOUR_CLUSTER_NAME
    location: YOUR_CLUSTER_LOCATION
```

This is the modern replacement for the older Travis steps because:

- it avoids manual SDK bootstrap scripting
- it avoids the older long-lived `service-account.json` default when Workload Identity Federation is available
- it configures `kubectl` for GKE directly through the official GKE credentials action

## Modern Notes With References

- The `google-github-actions/auth` action recommends Workload Identity Federation over service-account keys:
  https://github.com/google-github-actions/auth
- The `google-github-actions/get-gke-credentials` action is the official GitHub Action for configuring `kubectl` access to GKE:
  https://github.com/google-github-actions/get-gke-credentials
- The full durable GitHub Actions flow for this vault is tracked in [[github-actions-gke-deployment-flow]].

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The old Travis-based lesson is still useful for understanding the deployment prerequisites, but its exact implementation is dated.
- The course relies on a sensitive `service-account.json` file, while the modern preferred approach is short-lived GitHub Actions to Google Cloud authentication.
- The source explains how CI gets cloud credentials, but it still stops before showing the rest of the modern GitHub Actions deployment workflow in one file.
