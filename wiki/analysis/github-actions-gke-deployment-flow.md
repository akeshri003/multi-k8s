---
title: GitHub Actions GKE Deployment Flow
type: analysis
status: active
created: 2026-04-20
updated: 2026-04-22
tags:
  - github-actions
  - kubernetes
  - gke
  - ci
  - cd
  - docker
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/59 - The Deployment Process.md
  - wiki/sources/60 - CI Setup.md
  - wiki/sources/2026-04-22-mental-model-of-github-actions-gke-deployment.md
  - wiki/analysis/multi-k8s-google-cloud-ui-setup-for-github-actions.md
  - wiki/analysis/kubernetes-cloud-deployment-secret-management.md
---

# GitHub Actions GKE Deployment Flow

## Question

The course uses Travis CI for deployment. What is the modern GitHub Actions based equivalent for building, testing, pushing images, and deploying to GKE?

## Short Answer

The modern GitHub Actions version of the course flow is:

1. check out the repo
2. authenticate to Google Cloud
3. get GKE credentials for `kubectl`
4. log in to Docker Hub
5. build and run tests
6. build and push versioned images
7. apply the `k8s/` manifests
8. update deployments to the exact image tags that were just pushed

The overall deployment idea is the same as the course.

What changes is the CI platform and the implementation details:

- Travis CI becomes GitHub Actions
- Google Cloud SDK bootstrap steps can be replaced by official GitHub Actions
- Docker build/login steps can be replaced by official Docker actions

## Why This Is The Right Modern Translation

The course pipeline has two important responsibilities:

- CI: build and test the app
- CD: push images and update the Kubernetes cluster

That logic is still correct.

The modern translation is mainly about using current tooling:

- `google-github-actions/auth` for Google Cloud auth
- `google-github-actions/get-gke-credentials` for cluster access
- `docker/login-action` for Docker Hub auth
- `docker/build-push-action` for building and pushing images

## Recommended Authentication Pattern

The `google-github-actions/auth` README says Workload Identity Federation is recommended over long-lived service-account keys.

So the preferred modern setup is:

- GitHub Actions OIDC
- Google Cloud Workload Identity Federation
- short-lived credentials instead of a static JSON key

## Fallback: Service Account Key JSON

The same official `auth` action also supports `credentials_json`.

So if you cannot use Workload Identity Federation yet, a service-account JSON key can still be used as a fallback.

But for this vault, the right interpretation is:

- supported does not mean preferred
- JSON keys should be treated as sensitive long-lived credentials
- Workload Identity Federation should remain the target design

## Recommended Image Strategy

Do not rely on mutable `latest` alone.

Use a unique tag per build, such as:

```text
${{ github.sha }}
```

Then update the deployments to those exact pushed tags.

That makes the deployment step deterministic.

## Minimal Workflow Skeleton

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
          workload_identity_provider: YOUR_WORKLOAD_IDENTITY_PROVIDER
          service_account: YOUR_SERVICE_ACCOUNT

      - uses: google-github-actions/get-gke-credentials@v3
        with:
          cluster_name: YOUR_CLUSTER_NAME
          location: YOUR_CLUSTER_LOCATION

      - uses: docker/login-action@v4
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build test image
        run: docker build -t multi-client-test -f ./client/Dockerfile.dev ./client

      - name: Run tests
        run: docker run --rm -e CI=true multi-client-test npm test

      - name: Build and push client
        uses: docker/build-push-action@v7
        with:
          context: ./client
          push: true
          tags: YOUR_DOCKER_ID/multi-client:${{ github.sha }}

      - name: Build and push server
        uses: docker/build-push-action@v7
        with:
          context: ./server
          push: true
          tags: YOUR_DOCKER_ID/multi-server:${{ github.sha }}

      - name: Build and push worker
        uses: docker/build-push-action@v7
        with:
          context: ./worker
          push: true
          tags: YOUR_DOCKER_ID/multi-worker:${{ github.sha }}

      - name: Apply manifests
        run: kubectl apply -f k8s

      - name: Update deployment images
        run: |
          kubectl set image deployment/client-deployment client=YOUR_DOCKER_ID/multi-client:${GITHUB_SHA}
          kubectl set image deployment/server-deployment server=YOUR_DOCKER_ID/multi-server:${GITHUB_SHA}
          kubectl set image deployment/worker-deployment worker=YOUR_DOCKER_ID/multi-worker:${GITHUB_SHA}
```

## How To Read The Workflow

### Checkout

```yaml
- uses: actions/checkout@v4
```

This makes the repository available to the job.

### Google Cloud Auth

```yaml
- uses: google-github-actions/auth@v3
```

This authenticates the workflow to Google Cloud.

### GKE Credentials

```yaml
- uses: google-github-actions/get-gke-credentials@v3
```

This configures `kubectl` so it can talk to the cluster.

### Docker Hub Login

```yaml
- uses: docker/login-action@v4
```

The Docker login action documentation says to use a Docker Hub personal access token rather than an account password.

### Build And Push

```yaml
- uses: docker/build-push-action@v7
```

This builds and pushes images cleanly inside GitHub Actions.

### Apply And Update

```bash
kubectl apply -f k8s
kubectl set image ...
```

This keeps the cluster manifests in sync and then makes the deployments adopt the newly built image tags.

## Where The Workflow Actually Runs

The runner and the cluster are different execution environments.

- `runs-on: ubuntu-latest` means GitHub spins up a temporary Ubuntu VM in GitHub's own runner infrastructure.
- `actions/checkout` clones the repo onto that runner's local disk for the duration of the job.
- `google-github-actions/auth` gives that runner short-lived Google Cloud credentials.
- `google-github-actions/get-gke-credentials` writes kubeconfig on the runner so `kubectl` can talk to the GKE API server.

So the runner is a disposable control client, not part of GKE itself.

## What `kubectl` Is Actually Doing

`kubectl` is a client, not the place where Kubernetes work happens.

When the workflow runs `kubectl apply -f k8s`, the runner:

1. reads the YAML locally
2. packages it into API requests
3. sends those requests to the GKE control plane

After that, the real Kubernetes work happens inside GKE:

- the API server accepts the desired state
- controllers reconcile that desired state
- nodes pull images
- pods start running in the cluster

This separation matters because it explains why "deploying to GKE" really means "a GitHub-hosted runner talks to the GKE API."

## Practical Takeaway

For this vault, the modern deployment story is:

- keep the course's build-test-push-apply-update logic
- replace Travis with GitHub Actions
- use Workload Identity Federation when possible
- use versioned image tags
- treat `kubectl apply` and deployment image updates as separate steps

## Project Specific Setup Note

For the `multi-k8s` project, the detailed Google Cloud UI walkthrough for:

- verifying the provider
- binding the GitHub repo to the service account
- granting the service account GKE permissions
- testing the minimal workflow

now lives in [[multi-k8s-google-cloud-ui-setup-for-github-actions]].

## Citations And References

- GitHub Docs: Publishing Docker images in GitHub Actions
  https://docs.github.com/en/actions/use-cases-and-examples/publishing-packages/publishing-docker-images
- `google-github-actions/auth`:
  https://github.com/google-github-actions/auth
- `google-github-actions/get-gke-credentials`:
  https://github.com/google-github-actions/get-gke-credentials
- `docker/login-action`:
  https://github.com/docker/login-action
- `docker/build-push-action`:
  https://github.com/docker/build-push-action
