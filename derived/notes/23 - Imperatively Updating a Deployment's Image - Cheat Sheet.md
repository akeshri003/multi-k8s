# Imperatively Updating a Deployment's Image Cheat Sheet

## Source

- Transcript: `raw/transcripts/23 - Imperatively Updating a Deployment's Image.md`
- Wiki source page: [[23 - Imperatively Updating a Deployment's Image]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 5.42.59 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 5.47.49 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 5.49.33 PM.png]]

## Core Idea

This lesson gives the actual working recipe for the workaround from the last lesson:

1. build a version-tagged image
2. push it to Docker Hub
3. run `kubectl set image` to update the deployment

## Main Commands

```bash
docker build -t <docker-id>/multi-client:v5 .
docker push <docker-id>/multi-client:v5
kubectl set image deployment/client-deployment client=<docker-id>/multi-client:v5
kubectl get pods
minikube ip
```

## How To Read `kubectl set image`

```bash
kubectl set image deployment/client-deployment client=<docker-id>/multi-client:v5
```

- `deployment/client-deployment`
  the object being updated
- `client`
  the container name inside the pod template
- `<docker-id>/multi-client:v5`
  the new image reference

## How You Know It Worked

- `kubectl get pods` shows a very new pod age
- the browser eventually shows:

```text
Fib calculator version two
```

## Cache Caveat

If the browser does not show the new text immediately:

- wait a few seconds and refresh
- disable cache in DevTools
- rerun the `kubectl set image` command if needed

## Retrieval Cues

- "tag image with real version"
- "`kubectl set image` updates deployment image property"
- "new pod age proves rollout happened"
- "browser text proves new image reached the app"
