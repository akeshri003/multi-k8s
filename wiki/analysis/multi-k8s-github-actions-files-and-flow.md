---
title: Multi K8s GitHub Actions Files and Flow
type: analysis
status: active
created: 2026-04-20
updated: 2026-04-20
tags:
  - github-actions
  - kubernetes
  - gke
  - ci
  - cd
  - workflow
related:
  - wiki/topics/kubernetes.md
  - wiki/analysis/github-actions-gke-deployment-flow.md
  - wiki/analysis/multi-k8s-google-cloud-ui-setup-for-github-actions.md
---

# Multi K8s GitHub Actions Files and Flow

## Question

Where should the GitHub Actions YAML files live for `multi-k8s`? Should the deploy YAML be added into the existing CI file, or should it live in a separate file? And what do the current `ci.yaml` and `deploy.yaml` files actually mean?

## Short Answer

The files should live in:

```text
.github/workflows/
```

For this project, the clean setup is:

- `.github/workflows/ci.yaml`
- `.github/workflows/deploy.yaml`

So your assumption was correct:

```text
.github/workflows/deploy.yaml
```

is the right location for the deployment workflow.

## Why These Files Live Under `.github/workflows/`

GitHub Actions only treats YAML files as workflows if they are inside:

```text
.github/workflows/
```

That folder is special.

GitHub scans it automatically and turns each YAML file in it into a workflow.

So:

- a file in `.github/workflows/ci.yaml` becomes a workflow
- a file in `.github/workflows/deploy.yaml` becomes another workflow

If the same YAML lived somewhere else, GitHub would not run it as a workflow.

## Should CI And Deploy Be One File Or Two?

For this project, use two files.

### Recommended Split

- `ci.yaml` for validation
- `deploy.yaml` for cloud deployment

### Why This Split Is Better

Because these workflows have different purposes:

#### `ci.yaml`

This answers:

- does the code install?
- do the tests pass?
- do the basic build and syntax checks pass?

This should run often:

- on pull requests
- on pushes

#### `deploy.yaml`

This answers:

- after code is already considered good, should we push images and update the cluster?

This should be much more controlled:

- usually only on pushes to `main`
- sometimes manually

### Why Not Put Everything In One File?

You *can*, but it becomes harder to reason about:

- one workflow ends up doing validation and production deployment
- more secrets and permissions are loaded even when you only want tests
- beginners have a harder time debugging because “the pipeline” becomes one big mixed process

For learning and maintenance, two files are clearer.

## The Right Mental Model

Think of the split like this:

- `ci.yaml` = "is this code safe to merge?"
- `deploy.yaml` = "now that it is merged, should we publish and release it?"

That is a very common and healthy separation.

## Your Current `deploy.yaml`

You currently have:

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
          cluster_name: autopilot-cluster-1
          location: us-central1
          project_id: multi-k8s-493910

      - name: Verify cluster access
        run: |
          kubectl cluster-info
          kubectl get nodes
```

## What Each Part Means In `deploy.yaml`

### Workflow Name

```yaml
name: deploy
```

This is just the label GitHub shows in the Actions UI.

It does not affect cloud auth or deployment logic.

### Trigger

```yaml
on:
  push:
    branches:
      - main
```

This means:

- only run this workflow after code is pushed to `main`

That is a good deployment trigger for a beginner setup.

It prevents accidental cloud deployment on every feature branch.

### Job Name

```yaml
jobs:
  deploy:
```

This defines one job called `deploy`.

A job is a unit of work that runs on a fresh runner.

### Runner

```yaml
runs-on: ubuntu-latest
```

This tells GitHub to start a Linux VM for the job.

That VM is temporary.

Every run starts from a clean machine.

### Permissions

```yaml
permissions:
  id-token: write
  contents: read
```

This is very important.

- `contents: read` lets the workflow read the repository contents
- `id-token: write` lets GitHub mint an OIDC token for this run

That OIDC token is what makes Workload Identity Federation possible.

Without `id-token: write`, the Google auth step will not work correctly.

### Checkout

```yaml
- uses: actions/checkout@v4
```

This downloads your repository into the runner.

Without this step:

- your `k8s/` directory would not exist on the runner
- your app source code would not exist on the runner

### Google Cloud Auth

```yaml
- uses: google-github-actions/auth@v3
```

This step exchanges the GitHub OIDC identity for Google Cloud credentials.

It does **not** yet connect to the Kubernetes cluster by itself.

It just answers:

- who is this workflow in Google Cloud terms?

### Get GKE Credentials

```yaml
- uses: google-github-actions/get-gke-credentials@v3
```

This step configures `kubectl` so that it can talk to your GKE cluster.

So:

- `auth` = get Google Cloud identity
- `get-gke-credentials` = connect `kubectl` to the specific cluster

That distinction is important.

### Smoke Test

```yaml
- name: Verify cluster access
  run: |
    kubectl cluster-info
    kubectl get nodes
```

This is a very good first checkpoint.

It verifies:

- auth worked
- cluster selection worked
- permissions are good enough

So your current `deploy.yaml` is in a good state for an early smoke test.

## Your Current `ci.yaml`

You currently have a separate CI workflow with:

- `client` job
- `server` job
- `worker` job

That is a good structure.

## What `ci.yaml` Is Doing

### Triggers

```yaml
on:
  pull_request:
  push:
```

This means CI runs:

- when a PR is opened or updated
- when code is pushed

That is exactly what a validation workflow should do.

### Concurrency

```yaml
concurrency:
  group: ci-${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

This prevents old duplicate CI runs from wasting time.

If you push again quickly to the same branch, the older run gets cancelled.

That is a nice quality-of-life feature.

### Separate Jobs

The jobs are split by app component:

- `client`
- `server`
- `worker`

That is useful because:

- failures are isolated
- each service can have its own checks
- the Actions UI shows exactly which component failed

### Client Job

The `client` job does:

1. checkout
2. install Node
3. install dependencies
4. run tests
5. build the frontend

That is appropriate because the client has both:

- tests
- a real production build output

### Server Job

The `server` job does:

1. checkout
2. install Node
3. install dependencies
4. run `node --check` on source files

That is a lightweight syntax-validation job.

It is not a full test suite, but it still catches broken JavaScript syntax.

### Worker Job

The `worker` job is the same kind of lightweight syntax check for the worker service.
## Practical Takeaway

For `multi-k8s`, the clean answer is:

- store workflows in `.github/workflows/`
- keep `ci.yaml` and `deploy.yaml` separate
- use `ci.yaml` for validation
- use `deploy.yaml` for real cloud deployment
- your current `deploy.yaml` location is correct
- your current `deploy.yaml` content is a good first smoke test

## Related Notes

- [[github-actions-gke-deployment-flow]]
- [[multi-k8s-google-cloud-ui-setup-for-github-actions]]
