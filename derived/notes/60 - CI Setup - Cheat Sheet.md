# CI Setup Cheat Sheet

## Source

- Transcript: `raw/transcripts/60 - CI Setup.md`
- Wiki source page: [[60 - CI Setup]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 4.42.10 PM.png]]

## Course Pipeline

- install Google Cloud SDK
- authenticate to Google Cloud
- log in to Docker
- build test image
- run tests
- if tests pass, build and push all app images
- apply `k8s/` configs
- imperatively update deployment images

## Why The Last Step Exists

- reapplying manifests alone does not guarantee deployments pick up new image contents

## GitHub Actions Equivalent

For a modern GitHub Actions workflow, the same flow becomes:

1. `actions/checkout@v4`
2. `google-github-actions/auth@v3`
3. `google-github-actions/get-gke-credentials@v3`
4. `docker/login-action@v4`
5. build and run tests
6. `docker/build-push-action@v7` for each image
7. `kubectl apply -f k8s`
8. `kubectl set image ...` with the pushed tags

## Minimal Workflow Shape

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
```

See also: [[github-actions-gke-deployment-flow]]
