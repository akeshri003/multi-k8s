---
title: GitHub Actions YAML Patterns for CI CD
type: analysis
status: active
created: 2026-04-20
updated: 2026-04-20
tags:
  - github-actions
  - ci
  - cd
  - workflow
  - yaml
  - kubernetes
related:
  - wiki/topics/kubernetes.md
  - wiki/analysis/multi-k8s-github-actions-files-and-flow.md
  - wiki/analysis/github-actions-gke-deployment-flow.md
---

# GitHub Actions YAML Patterns for CI CD

## Question

How should a beginner think about writing GitHub Actions YAML files for CI and CD? What is the general pattern, and how does that pattern map onto the `multi-k8s` examples?

## Short Answer

A GitHub Actions workflow usually follows this pattern:

1. decide **when** it should run
2. decide **what kind of job** it is
3. decide **what machine** it needs
4. decide **what permissions** it needs
5. define the **steps** in the correct order

That sounds simple, but it is the right way to think.

Instead of memorizing YAML syntax first, start with the job's purpose.

## First Principles

A workflow file is just an automation recipe.

Each file answers five questions:

### 1. When should this run?

That is the `on:` block.

Examples:

- on every pull request
- on every push
- only on pushes to `main`
- manually

### 2. What is this workflow trying to prove or do?

This is the conceptual purpose:

- CI: prove the code is valid
- CD: publish and deploy the code

That purpose should shape the whole file.

### 3. What environment does it need?

That is usually:

```yaml
runs-on: ubuntu-latest
```

Sometimes the environment also includes:

- Docker
- Node
- Python
- cloud credentials

### 4. What permissions does it need?

Good workflows ask for the minimum needed.

Example:

```yaml
permissions:
  contents: read
  id-token: write
```

### 5. What are the exact steps, in order?

This is the `steps:` list.

Order matters.

For example:

- checkout before build
- auth before cloud access
- build before deploy

## Pattern 1: CI Workflow

A CI workflow usually asks:

- can the code be installed?
- can it be tested?
- can it be built?

That means the pattern is usually:

```yaml
name: CI

on:
  pull_request:
  push:

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - checkout
      - set up language runtime
      - install dependencies
      - run tests
      - run build
```

## Pattern 2: CD Workflow

A CD workflow usually asks:

- can I authenticate?
- can I build release artifacts?
- can I publish them?
- can I update the running system?

That means the pattern is usually:

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
      contents: read
      id-token: write
    steps:
      - checkout
      - authenticate
      - connect to target environment
      - build artifacts
      - publish artifacts
      - apply deployment changes
```

## Mapping This To `multi-k8s`

The `multi-k8s` project is a good example because it has both kinds of workflows.

### `ci.yaml`

Its purpose is:

- validate code

So its pattern is:

```text
trigger -> runner -> checkout -> install -> test/check -> build
```

That is why your current `ci.yaml` has:

- `pull_request` and `push`
- separate jobs for `client`, `server`, and `worker`
- Node setup
- dependency install
- tests or syntax checks

### `deploy.yaml`

Its purpose is:

- authenticate to Google Cloud
- connect to GKE
- later build/push/deploy

So its pattern is:

```text
trigger on main -> checkout -> auth -> cluster access -> smoke test -> later deploy
```

That is why your current `deploy.yaml` has:

- only `push` to `main`
- `id-token: write`
- `google-github-actions/auth`
- `get-gke-credentials`
- `kubectl cluster-info`

## How To Think About Writing One From Scratch

Use this train of thought:

### Step A: Name The Purpose

Ask:

- is this validation?
- or is this deployment?

If you mix those too early, the file becomes harder to reason about.

### Step B: Choose The Trigger

Ask:

- should this happen on every PR?
- only on `main`?
- manually?

Examples:

- validation: `pull_request` and `push`
- deploy: `push` to `main`

### Step C: List The Required Resources

Ask:

- do I need source code?
- do I need Node?
- do I need Docker?
- do I need cloud auth?
- do I need `kubectl`?

This decides your setup steps.

### Step D: Write The Steps In Dependency Order

Always ask:

- what must exist before the next thing can happen?

For example:

- source code before build
- auth before cluster access
- image build before image push
- image push before deployment update

### Step E: Keep The First Version Small

Do not write the full final pipeline in one shot.

For example, your current `deploy.yaml` is good because it starts with:

- checkout
- auth
- get cluster credentials
- smoke test

That is much easier to debug than immediately adding image builds and production deployment.

## Pattern Template: CI

Here is a general CI skeleton:

```yaml
name: CI

on:
  pull_request:
  push:

jobs:
  validate:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 18

      - run: npm install

      - run: npm test

      - run: npm run build
```

## Pattern Template: CD

Here is a general CD skeleton:

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
      contents: read
      id-token: write

    steps:
      - uses: actions/checkout@v4

      - uses: google-github-actions/auth@v3
        with:
          project_id: YOUR_PROJECT_ID
          workload_identity_provider: YOUR_PROVIDER
          service_account: YOUR_SERVICE_ACCOUNT

      - uses: google-github-actions/get-gke-credentials@v3
        with:
          cluster_name: YOUR_CLUSTER
          location: YOUR_LOCATION

      - run: kubectl get nodes
```

## Why YAML Looks The Way It Does

YAML is just a nested data structure.

In GitHub Actions:

- top-level keys define the workflow
- `jobs:` contains one or more jobs
- each job contains machine info and steps
- each step is one action or one shell command

So the structure is:

```text
workflow
  -> triggers
  -> jobs
    -> runner
    -> permissions
    -> steps
```

That is the core mental model.

## The Most Useful Beginner Rule

When writing or reading a workflow, always ask:

```text
What is this workflow trying to prove or change?
```

If the answer is:

- "prove code quality" -> CI
- "change running infrastructure" -> CD

then the rest of the file should reflect that purpose.

## Practical Takeaway

For `multi-k8s`, the safest pattern is:

- one workflow for validation
- one workflow for deployment
- keep each file small at first
- add complexity only after the previous checkpoint works

That is the right way to learn these files and the right way to maintain them.

## Related Notes

- [[multi-k8s-github-actions-files-and-flow]]
- [[github-actions-gke-deployment-flow]]
- [[multi-k8s-google-cloud-ui-setup-for-github-actions]]


projects/871393691181/locations/global/workloadIdentityPools/github-pool-multi-k8s/providers/github-provider-multi-k8s