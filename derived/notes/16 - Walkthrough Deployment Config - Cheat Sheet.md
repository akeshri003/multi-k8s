# Walkthrough Deployment Config Cheat Sheet

## Source

- Transcript: `raw/transcripts/16 - Walkthrough Deployment Config.md`
- Wiki source page: [[16 - Walkthrough Deployment Config]]
- Key visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.38.36 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.41.19 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.41.34 PM.png]]

## Core Idea

This lesson explains what each important section of the first deployment manifest is doing.

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

## What The Important Parts Mean

- `metadata.name`
  Name of the deployment object.
- `template`
  The pod recipe used for every pod the deployment creates.
- `replicas: 1`
  Keep one matching pod running.
- `selector.matchLabels`
  Find the pods that belong to this deployment.
- `template.metadata.labels`
  Provide the labels the selector will match against.

## Most Important Mental Model

- deployment creates pods
- template defines those pods
- labels mark those pods
- selector finds those pods again later

## Why The Label Appears Twice

It looks repetitive, but the lesson says that is intentional:

- the pod needs the label
- the deployment needs a selector that matches that label

## Retrieval Cues

- "`template` is the pod configuration"
- "`replicas` is pod count"
- "`selector` is how deployment finds its pods"
- "the duplicated label is deliberate"

