# Updating Deployment Images Cheat Sheet

## Source

- Transcript: `raw/transcripts/20 - Updating Deployment Images.md`
- Wiki source page: [[20 - Updating Deployment Images]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 5.23.36 PM.png]]

## Core Idea

This lesson resets the deployment into a simple state before tackling image-version updates.

The deployment is changed so that:

- it uses `multi-client` again
- it runs only `1` replica
- it exposes `containerPort: 3000` again

That makes browser testing easy before the image-update experiment starts.

## Main Commands

```bash
kubectl apply -f client-deployment.yaml
minikube ip
```

## Three-Step Plan

1. change the deployment back to `multi-client`
2. rebuild and push a changed `multi-client` image to Docker Hub
3. somehow get the deployment to recreate its pod with that latest image

## Why `multi-client` Returns Here

- it is easy to verify in the browser
- a visible page makes version changes obvious
- one replica makes pod replacement easier to observe

## Retrieval Cues

- "reset deployment to multi-client"
- "set replicas back to one"
- "restore port 3000"
- "prepare for image update problem"
