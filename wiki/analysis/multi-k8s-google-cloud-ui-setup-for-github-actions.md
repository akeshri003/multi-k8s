---
title: Multi K8s Google Cloud UI Setup for GitHub Actions
type: analysis
status: active
created: 2026-04-20
updated: 2026-04-20
tags:
  - github-actions
  - kubernetes
  - gke
  - gcp
  - workload-identity-federation
  - setup
related:
  - wiki/topics/kubernetes.md
  - wiki/analysis/github-actions-gke-deployment-flow.md
  - wiki/sources/60 - CI Setup.md
  - wiki/sources/61 - Installing google cloud sdk.md
  - wiki/sources/62 - Service Account Steps for GCP.md
---

# Multi K8s Google Cloud UI Setup for GitHub Actions

## Question

For the `multi-k8s` project, what are the detailed next steps after creating the Workload Identity Pool, provider, and service account, and how do those steps map to the Google Cloud UI?

## Current Known Values

These are the values already established in the project setup:

- project ID: `multi-k8s-493910`
- project number: `871393691181`
- workload identity pool: `github-pool-multi-k8s`
- workload identity provider:
  `projects/871393691181/locations/global/workloadIdentityPools/github-pool-multi-k8s/providers/gh-actions-provider-multi-k8s`
- service account:
  `gh-actions-multi-k8s@multi-k8s-493910.iam.gserviceaccount.com`

The GitHub Actions auth block being prepared is:

```yaml
jobs:
  deploy:
    permissions:
      id-token: write
      contents: read

    steps:
      - uses: google-github-actions/auth@v2
        with:
          workload_identity_provider: 'projects/871393691181/locations/global/workloadIdentityPools/github-pool-multi-k8s/providers/gh-actions-provider-multi-k8s'
          service_account: 'gh-actions-multi-k8s@multi-k8s-493910.iam.gserviceaccount.com'
```

## Recommended Update To The Workflow Snippet

Use `auth@v3`, not `auth@v2`, and add checkout first:

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    permissions:
      id-token: write
      contents: read

    steps:
      - uses: actions/checkout@v4

      - uses: google-github-actions/auth@v3
        with:
          project_id: multi-k8s-493910
          workload_identity_provider: projects/871393691181/locations/global/workloadIdentityPools/github-pool-multi-k8s/providers/gh-actions-provider-multi-k8s
          service_account: gh-actions-multi-k8s@multi-k8s-493910.iam.gserviceaccount.com
```

## Step 1: Verify The Workload Identity Provider

Before editing IAM bindings, verify the provider itself.

### In The Google Cloud UI

1. Open `IAM & Admin`
2. Open `Workload Identity Federation`
3. Open the pool:
   `github-pool-multi-k8s`
4. Open the provider:
   `gh-actions-provider-multi-k8s`

### What To Check

- the issuer is GitHub OIDC
- the attribute mapping includes at least:
  - `google.subject=assertion.sub`
  - `attribute.repository=assertion.repository`
  - `attribute.ref=assertion.ref`
  - `attribute.actor=assertion.actor`
- if there is an attribute condition, it should restrict access to the correct repository

Example repository condition:

```text
assertion.repository=='YOUR_GITHUB_OWNER/YOUR_REPO'
```

If `attribute.repository` is not mapped, the service-account trust step will not work correctly.

## Step 2: Allow The GitHub Repo To Impersonate The Service Account

This is the UI equivalent of granting:

```text
roles/iam.workloadIdentityUser
```

### In The Google Cloud UI

1. Open `IAM & Admin`
2. Open `Service Accounts`
3. Find:
   `gh-actions-multi-k8s@multi-k8s-493910.iam.gserviceaccount.com`
4. Click the service account email
5. Open the `Permissions` tab
6. Find `Principals with access to this service account`
7. Click `Grant access`

### Principal To Add

In `New principals`, paste:

```text
principalSet://iam.googleapis.com/projects/871393691181/locations/global/workloadIdentityPools/github-pool-multi-k8s/attribute.repository/YOUR_GITHUB_OWNER/YOUR_REPO
```

Example if the repo slug is `aryankeshri/multi-k8s`:

```text
principalSet://iam.googleapis.com/projects/871393691181/locations/global/workloadIdentityPools/github-pool-multi-k8s/attribute.repository/aryankeshri/multi-k8s
```

### Role To Add

In `Select a role`, search for:

```text
Workload Identity User
```

Choose:

```text
Workload Identity User (roles/iam.workloadIdentityUser)
```

Then click `Save`.

### What This Means

This says:

- this specific GitHub repository is allowed to impersonate this specific Google Cloud service account

### What To Verify

On the service account `Permissions` tab, you should see:

- principal: the long `principalSet://...attribute.repository/...` string
- role: `Workload Identity User`

## Step 3: Grant The Service Account Permission To Deploy To GKE

This is separate from Step 2.

Step 2 answers:

- who is allowed to become the service account

Step 3 answers:

- what that service account is allowed to do

### In The Google Cloud UI

1. Open `IAM & Admin`
2. Open `IAM`
3. Make sure the selected project is:
   `multi-k8s-493910`
4. Click `Grant access`

### Principal To Add

In `New principals`, enter:

```text
gh-actions-multi-k8s@multi-k8s-493910.iam.gserviceaccount.com
```

### Role To Add

In `Select a role`, search for:

```text
Kubernetes Engine Admin
```

Choose:

```text
Kubernetes Engine Admin (roles/container.admin)
```

Then click `Save`.

### What This Means

This gives the service account broad permission to manage the GKE cluster and Kubernetes resources through GKE.

For a beginner setup, this is the simplest working role.

It can be tightened later after the deployment path works end to end.

## Step 4: Add GKE Credentials To The Workflow

After auth succeeds, configure `kubectl` for the cluster.

```yaml
- uses: google-github-actions/get-gke-credentials@v3
  with:
    cluster_name: YOUR_CLUSTER_NAME
    location: YOUR_CLUSTER_LOCATION
    project_id: multi-k8s-493910
```

Important note:

- for GKE Autopilot, `location` is often a region such as `us-central1`, not a zone

## Step 5: Run A Minimal Smoke Test First

Before adding Docker builds and deployment steps, test only auth and cluster access.

Add this first:

```yaml
- name: Verify cluster access
  run: |
    kubectl cluster-info
    kubectl get nodes
```

This is the best first checkpoint.

If it works, then:

- GitHub OIDC trust is correct
- service-account impersonation is correct
- the service account has enough permission to talk to GKE

## Step 6: Only Then Add Build, Push, And Deploy

Once auth plus cluster access works, extend the workflow in this order:

1. Docker Hub login
2. test image build
3. test run
4. production image build and push
5. `kubectl apply -f k8s`
6. `kubectl set image ...`

That order makes debugging much easier.

## Minimal Full Shape

```yaml
name: deploy

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    permissions:
      id-token: write
      contents: read

    steps:
      - uses: actions/checkout@v4

      - uses: google-github-actions/auth@v3
        with:
          project_id: multi-k8s-493910
          workload_identity_provider: projects/871393691181/locations/global/workloadIdentityPools/github-pool-multi-k8s/providers/gh-actions-provider-multi-k8s
          service_account: gh-actions-multi-k8s@multi-k8s-493910.iam.gserviceaccount.com

      - uses: google-github-actions/get-gke-credentials@v3
        with:
          cluster_name: YOUR_CLUSTER_NAME
          location: YOUR_CLUSTER_LOCATION
          project_id: multi-k8s-493910

      - name: Verify cluster access
        run: |
          kubectl cluster-info
          kubectl get nodes
```

## Common Mistakes

- using `auth@v2` instead of `auth@v3`
- forgetting `actions/checkout@v4`
- using project ID where the principal string needs the project number
- using the wrong GitHub repo slug in the `principalSet`
- granting `Workload Identity User` on the project instead of on the service account
- granting GKE roles to the GitHub principal instead of to the service account
- using the wrong cluster location format
- editing IAM resources in the wrong project because the top project selector is wrong

## What To Add In GitHub

For the minimal auth test, no GCP JSON key secret is needed.

Later, for full deploy, add:

- variable: `DOCKERHUB_USERNAME`
- secret: `DOCKERHUB_TOKEN`

You can also store these as repo variables for clarity:

- `GCP_PROJECT_ID`
- `GKE_CLUSTER_NAME`
- `GKE_CLUSTER_LOCATION`
- `GCP_WIF_PROVIDER`
- `GCP_SERVICE_ACCOUNT`

## Practical Order To Follow

1. verify the provider mappings
2. grant the GitHub repo principal `Workload Identity User` on the service account
3. grant the service account `Kubernetes Engine Admin` on the project
4. add `checkout -> auth -> get-gke-credentials -> kubectl get nodes`
5. push and confirm the workflow succeeds
6. only then add Docker build, push, and deploy logic

## References

- GitHub OIDC with Google Cloud:
  https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-google-cloud-platform
- `google-github-actions/auth`:
  https://github.com/google-github-actions/auth
- `google-github-actions/get-gke-credentials`:
  https://github.com/google-github-actions/get-gke-credentials
- Google Cloud Workload Identity Federation:
  https://cloud.google.com/iam/docs/workload-identity-federation
- GKE IAM roles:
  https://cloud.google.com/iam/docs/roles-permissions/container
