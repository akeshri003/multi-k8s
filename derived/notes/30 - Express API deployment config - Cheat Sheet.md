# Express API deployment config Cheat Sheet

## Source

- Transcript: `raw/transcripts/30 - Express API deployment config.md`
- Wiki source page: [[30 - Express API deployment config]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 8.45.18 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 8.46.02 PM.png]]

## Core Idea

This lesson creates the backend deployment for the Express API.

It is the server-side version of the earlier frontend deployment.

## Main YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: server-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      component: server
  template:
    metadata:
      labels:
        component: server
    spec:
      containers:
        - name: server
          image: stephengrider/multi-server
          ports:
            - containerPort: 5000
```

## What Changes From The Frontend Deployment

- name becomes `server-deployment`
- label becomes `component: server`
- container name becomes `server`
- image becomes `stephengrider/multi-server`
- container port becomes `5000`

## Why Port `5000`

- the Express API is hard-coded to listen on `5000`
- so the later service also needs to route traffic to `5000`

## Why This Is Still Internal

- the server should not be directly exposed with `NodePort`
- it should sit behind a `ClusterIP` service
- outside traffic will later come through `Ingress`

## Important Incomplete Part

- this deployment still needs environment variables
- those env vars will tell the API how to connect to Redis and Postgres

## Retrieval Cues

- "backend deployment mirrors frontend deployment"
- "`component: server`"
- "`multi-server` listens on `5000`"
- "still missing Redis/Postgres env vars"
