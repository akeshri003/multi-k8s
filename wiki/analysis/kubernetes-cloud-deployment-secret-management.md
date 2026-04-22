---
title: Kubernetes Cloud Deployment Secret Management
type: analysis
status: active
created: 2026-04-22
updated: 2026-04-22
tags:
  - kubernetes
  - secrets
  - gke
  - github-actions
  - gcp
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/48 - Creating an Encoded Secret.md
  - wiki/sources/49 - Postgres Environment Variable Fix.md
  - wiki/sources/66 - Configuring the gcloud CLI on the cloud console.md
  - wiki/sources/2026-04-22-managing-secrets-for-cloud-deployment.md
  - wiki/analysis/github-actions-gke-deployment-flow.md
---

# Kubernetes Cloud Deployment Secret Management

## Question

How should this repo think about getting deployment secrets such as `pgpassword` into the live Kubernetes cluster?

## Answer

The current wiki now supports a three-level answer.

For the course-aligned baseline, create the Kubernetes `Secret` once manually against the live cluster from Cloud Shell. That keeps the password out of git and out of GitHub Actions, and it matches the course's operational pattern.

For a more reproducible side-project workflow, store the password in GitHub Actions secrets and have the deployment workflow create or update the Kubernetes `Secret` during deploy.

For real production workloads, the strongest direction in the current source base is to keep the secret in GCP Secret Manager and sync it into the cluster through an operator such as External Secrets Operator.

So the practical recommendation is:

1. manual Cloud Shell secret creation for learning and immediate progress
2. GitHub Actions-managed secret creation if the repo needs reproducible cluster rebuilds
3. Secret Manager plus an operator for serious production use

## Evidence From The Wiki

- [[48 - Creating an Encoded Secret]] introduces the basic Kubernetes secret object and the imperative `kubectl create secret generic pgpassword --from-literal PGPASSWORD=postgres` workflow.
- [[49 - Postgres Environment Variable Fix]] shows that both the server and Postgres deployments now depend on the `pgpassword` secret through `valueFrom.secretKeyRef`.
- [[66 - Configuring the gcloud CLI on the cloud console]] makes the cloud-deployment consequence explicit: the remote cluster also needs that secret, so Cloud Shell is used to target the live cluster before creating it.
- [[2026-04-22-managing-secrets-for-cloud-deployment]] compares three concrete patterns:
  - manual Cloud Shell creation
  - GitHub Actions secret to Kubernetes secret creation
  - GCP Secret Manager plus External Secrets Operator
- [[github-actions-gke-deployment-flow]] already captures that the deployment workflow runs in GitHub Actions and talks to GKE through `kubectl`, which is what makes the second option mechanically possible.

## Uncertainties

- The wiki still does not define the final standard for this repo, so the guidance remains a ranked set of options rather than a locked-in implementation.
- The production-grade Secret Manager path is only described conceptually so far; there is no implementation note yet for External Secrets Operator or an equivalent controller.
- The exact threat model for this repo is not written down, so the relative suitability of manual creation versus GitHub Actions secrets is still partly contextual.

## Related Pages

- [[48 - Creating an Encoded Secret]]
- [[49 - Postgres Environment Variable Fix]]
- [[66 - Configuring the gcloud CLI on the cloud console]]
- [[2026-04-22-managing-secrets-for-cloud-deployment]]
- [[github-actions-gke-deployment-flow]]
