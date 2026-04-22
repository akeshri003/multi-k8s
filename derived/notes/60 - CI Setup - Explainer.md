# CI Setup Explainer

## Source

- Transcript: `raw/transcripts/60 - CI Setup.md`
- Wiki source page: [[60 - CI Setup]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 4.42.10 PM.png]]

## What This Lesson Is Doing

This lesson explains the automation layer that sits on top of the Kubernetes manifests.

The course uses Travis CI, but the deeper idea is independent of Travis:

CI/CD has to:

- authenticate to cloud infrastructure
- build and test the app
- publish new images
- apply cluster config
- force deployments to use the new image versions

## The Course's Travis Flow

The flow diagram is:

1. install Google Cloud SDK
2. configure Google Cloud auth
3. log in to Docker
4. build a test version of `multi-client`
5. run tests
6. if tests pass, run a deploy script
7. build and push all production images
8. apply every config in `k8s/`
9. imperatively update deployments to the newest image tags

That is the main operational story the lesson wants you to understand.

## Why The Pipeline Needs Both Apply And Set Image

This is the important Kubernetes-specific nuance.

Earlier lessons already showed that:

- `kubectl apply -f k8s` updates manifests
- but it does not automatically make a deployment pull a newer image if the image reference did not visibly change

So the pipeline still needs an explicit image-update step.

That is why the CI flow is not just:

- build
- push
- apply

It is:

- build
- push
- apply
- then update the deployments to the correct image tags

## The GitHub Actions Translation

Because you said you would actually use GitHub Actions, the modern equivalent is:

1. check out the code
2. authenticate to Google Cloud
3. fetch GKE credentials for `kubectl`
4. log in to Docker Hub
5. build and run tests
6. build and push versioned images
7. apply the `k8s/` directory
8. update deployments to the exact pushed image tags

## Practical GitHub Actions Steps

The official-action version looks like this:

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

- uses: docker/login-action@v4
  with:
    username: ${{ vars.DOCKERHUB_USERNAME }}
    password: ${{ secrets.DOCKERHUB_TOKEN }}
```

Then the workflow builds, tests, pushes images, applies manifests, and updates deployments.

## Why This Is Better Than Copying Travis Literally

GitHub Actions gives you:

- native GitHub integration
- purpose-built actions for Google Cloud auth and GKE credentials
- cleaner Docker login and build/push steps

So the durable lesson is:

- keep the course pipeline logic
- modernize the CI system and the auth/build actions

For the full durable version, including a larger workflow skeleton and the official references, see [[github-actions-gke-deployment-flow]].
