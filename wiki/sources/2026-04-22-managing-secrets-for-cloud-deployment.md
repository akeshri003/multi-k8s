---
title: Managing the secrets for cloud deployment
type: source
status: active
created: 2026-04-22
updated: 2026-04-22
source_path: raw/notes/Managing the secrets for Cloud Deployment..md
source_date: 2026-04-22
tags:
  - kubernetes
  - secrets
  - github-actions
  - gke
  - gcp
  - secret-manager
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/48 - Creating an Encoded Secret.md
  - wiki/sources/49 - Postgres Environment Variable Fix.md
  - wiki/sources/66 - Configuring the gcloud CLI on the cloud console.md
  - wiki/analysis/kubernetes-cloud-deployment-secret-management.md
---

# Managing the secrets for cloud deployment

## Metadata

- Source type: raw explanatory note capturing secret-management guidance from the recent cloud-deployment discussion
- Note quality: strong enough to preserve three concrete secret-management options, their tradeoffs, and the suggested progression for this repo
- Embedded visuals:
  - None

## Summary

This note starts from the concrete `pgpassword` secret dependency in the Postgres deployment and asks how that Kubernetes `Secret` should reach the live cluster. It lays out three options. The course-aligned starting point is to create the secret once manually with `kubectl create secret generic pgpassword ...` from Cloud Shell. The next step up is to store the password in GitHub Actions secrets and let the workflow create or update the Kubernetes `Secret`. The production-grade path is to store the secret in GCP Secret Manager and sync it into the cluster through an operator such as External Secrets Operator. The note's practical recommendation is to start manual for learning, then automate with GitHub secrets if needed, and reserve Secret Manager plus an operator for real production workloads.

## Key Claims

- The Postgres pod expects a Kubernetes `Secret` named `pgpassword` with key `PGPASSWORD`.
- If that secret does not exist, the pod can fail with `CreateContainerConfigError`.
- Manual creation through Cloud Shell is the simplest course-aligned answer:
  - create the secret once
  - keep the password out of GitHub and out of the repo
- GitHub Actions can create or update the secret from a GitHub repository secret, which improves reproducibility but moves the secret into GitHub's secret store.
- GCP Secret Manager plus External Secrets Operator is the most production-grade option because the secret never has to live in GitHub.
- A Kubernetes `Secret` manifest committed to git is not safe if it contains only base64-encoded secret data, because base64 is encoding rather than encryption.

## Important Evidence Or Examples

The note's manual secret-creation path is:

```bash
kubectl create secret generic pgpassword \
  --from-literal PGPASSWORD='your-real-password-here'
```

Its workflow-managed path is:

```yaml
- name: Ensure database secret exists
  env:
    PG_PASSWORD: ${{ secrets.PG_PASSWORD }}
  run: |
    kubectl create secret generic pgpassword \
      --from-literal PGPASSWORD="$PG_PASSWORD" \
      --dry-run=client -o yaml | kubectl apply -f -
```

The note's recommended progression is explicit:

1. use manual Cloud Shell secret creation now
2. upgrade later to GitHub Actions-managed secret creation
3. use Secret Manager plus an operator for real production workloads

## Related Entities

- None yet.

## Related Topics

- [[kubernetes]]

## Tensions Or Open Questions

- The note is advisory rather than official product documentation, especially around the recommendation hierarchy between manual creation, GitHub Actions secrets, and GCP Secret Manager.
- The repo still does not yet contain the final standardized secret-management path, so the note preserves options rather than a settled implementation.
- The note mentions External Secrets Operator as the production-grade direction, but the wiki does not yet contain an implementation note for that operator.
