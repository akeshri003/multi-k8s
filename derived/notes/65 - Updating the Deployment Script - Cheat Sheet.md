# Updating the Deployment Script Cheat Sheet

## Source

- Transcript: `raw/transcripts/65 - Updating the Deployment Script.md`
- Wiki source page: [[65 - Updating the Deployment Script]]

## New Travis Env Vars

```yaml
env:
  global:
    - SHA=$(git rev-parse HEAD)
    - CLOUDSDK_CORE_DISABLE_PROMPTS=1
```

## New Build Pattern

```bash
docker build -t DOCKER_ID/multi-client:latest -t DOCKER_ID/multi-client:$SHA -f ./client/Dockerfile ./client
```

Repeat for `multi-server` and `multi-worker`.

## Push Pattern

```bash
docker push DOCKER_ID/multi-client:latest
docker push DOCKER_ID/multi-client:$SHA
```

Repeat for all three images.

## Deployment Update Pattern

```bash
kubectl set image deployment/client-deployment client=DOCKER_ID/multi-client:$SHA
kubectl set image deployment/server-deployment server=DOCKER_ID/multi-server:$SHA
kubectl set image deployment/worker-deployment worker=DOCKER_ID/multi-worker:$SHA
```

## Durable Idea

- `latest` is the default
- `$SHA` is the exact rollout version

See also: [[github-actions-gke-deployment-flow]]
