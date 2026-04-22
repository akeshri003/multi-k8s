# Deployment Configuration files. Cheat Sheet

## Source

- Transcript: `raw/transcripts/15 - Deployment Configuration files..md`
- Wiki source page: [[15 - Deployment Configuration files.]]
- Key visual:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.35.07 PM.png]]

## Core Idea

This is the first real deployment manifest in the course.

Instead of creating a pod directly, the course now creates a deployment that creates and manages pods.

## Main YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: client-deployment
spec:
  replicas: 1
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

## What Each Part Means

- `apiVersion: apps/v1`
  Use the API group/version that contains `Deployment`.
- `kind: Deployment`
  Create a deployment object.
- `metadata.name: client-deployment`
  Name the deployment.
- `replicas: 1`
  Keep one pod running.
- `selector.matchLabels`
  Find the pods this deployment should manage.
- `template`
  Describe what each managed pod should look like.

## Why This Matters

- earlier lessons used a direct `Pod`
- this lesson switches to `Deployment`
- the deployment will now create pods for us

## Retrieval Cues

- "deployment replaces direct pod creation"
- "`apps/v1` plus `kind: Deployment`"
- "`replicas` controls pod count"
- "`template` is the pod recipe"

