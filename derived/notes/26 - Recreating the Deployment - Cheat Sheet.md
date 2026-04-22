# Recreating the Deployment Cheat Sheet

## Source

- Transcript: `raw/transcripts/26 - Recreating the Deployment.md`
- Wiki source page: [[26 - Recreating the Deployment]]

## Core Idea

This lesson starts the real Kubernetes migration for the `complex` app.

It does two main things:

1. cleans out old Elastic Beanstalk / Docker Compose deployment files
2. creates the first real Kubernetes manifest for the frontend deployment

## Project Cleanup

The lesson removes old pieces that are no longer needed:

- `.travis.yml`
- Docker Compose file
- Docker-run-related file(s)
- `nginx/` folder

Why:

- Kubernetes will replace the old deployment path
- `Ingress` will replace the old Nginx-based routing setup

## New Folder

```text
k8s/
```

This becomes the home for the Kubernetes manifest files.

## First Manifest

```text
k8s/client-deployment.yml
```

## Main Deployment Shape

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: client-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      component: web
  template:
    metadata:
      labels:
        component: web
    spec:
      containers:
        - name: client
          image: stephengrider/multi-client
          ports:
            - containerPort: 3000
```

## Important Meanings

- `replicas: 3`
  keep three frontend pods running
- `component: web`
  label/selector pair that ties the deployment to its pods
- `image: stephengrider/multi-client`
  frontend image to run
- `containerPort: 3000`
  port exposed inside the pod template

## Retrieval Cues

- "first real manifest in k8s folder"
- "client deployment recreated from scratch"
- "three replicas of the frontend"
- "old nginx folder removed because ingress is coming next"
