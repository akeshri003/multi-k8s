# The Worker Deployment Cheat Sheet

## Source

- Transcript: `raw/transcripts/33 - The Worker Deployment.md`
- Wiki source page: [[33 - The Worker Deployment]]

## Core Idea

This lesson creates the deployment for the worker process.

The worker is different from the client and server because nothing needs to call into it directly.

## Main YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: worker-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: worker
  template:
    metadata:
      labels:
        component: worker
    spec:
      containers:
        - name: worker
          image: stephengrider/multi-worker
```

## What Matters Most

- `replicas: 1`
  start with one worker pod
- `component: worker`
  this is how the deployment identifies its pods
- `image: stephengrider/multi-worker`
  run the worker image

## Why Only One Replica For Now

- the worker is the most likely thing to scale later
- it handles the expensive Fibonacci calculation
- but the course starts with one replica first

## Why There Is No Service

- nothing else is supposed to send direct requests into the worker
- the worker reaches out to other parts of the app instead
- so no service is needed
- no port config is needed either

## Important Incomplete Part

- the worker still needs Redis-related environment variables later

## Retrieval Cues

- "worker gets a deployment but no service"
- "`component: worker`"
- "no inbound traffic means no port and no service"
- "worker is the future scaling candidate"
