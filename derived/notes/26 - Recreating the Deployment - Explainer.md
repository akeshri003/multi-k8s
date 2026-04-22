# Recreating the Deployment Explainer

## Source

- Transcript: `raw/transcripts/26 - Recreating the Deployment.md`
- Wiki source page: [[26 - Recreating the Deployment]]

## What This Lesson Starts

The previous lesson gave the map.

This lesson starts the actual migration work.

That is an important shift:

- before, the course was still describing the architecture
- now, it begins rewriting the `complex` app around Kubernetes manifests

## Step 1: Clean Up The Old Project

The lesson begins by removing files and folders tied to the older deployment paths.

That includes:

- `.travis.yml`
- Docker Compose file
- Docker-run-related file(s)
- `nginx/` folder

The reasoning is straightforward:

- Kubernetes is now going to be used for both development and production
- the old Compose / Elastic Beanstalk deployment files are no longer the main path
- routing will move away from the custom Nginx image and toward `Ingress`

This is the first time the migration feels irreversible.

Until now, the course was mostly comparing ideas.

Now it is actively reshaping the project directory around Kubernetes.

## Step 2: Create A `k8s/` Folder

The lesson then creates:

```text
k8s/
```

This is where the new manifest files will live.

That folder is important because it turns the Kubernetes architecture into an actual file-based codebase rather than just a diagram.

The course also says there will be around eleven manifest files by the end.

That helps set expectations:

this is not a one-file migration.

## Step 3: Recreate The Frontend Deployment First

The first real manifest is:

```text
k8s/client-deployment.yml
```

This file recreates the frontend as a deployment.

The normalized structure is:

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

## What Each Part Means

### `apiVersion: apps/v1`

This is the API group/version used for `Deployment`.

### `kind: Deployment`

This says the file creates a deployment, not a pod or service.

### `metadata.name: client-deployment`

This is the object name Kubernetes will use for the deployment.

### `replicas: 3`

The frontend should run as three pods, not just one.

That is an important shift from the smaller earlier examples.

The app is now being built as a more realistic multi-copy service.

### `selector.matchLabels`

This is how the deployment knows which pods belong to it.

### `template.metadata.labels`

These labels are placed onto the pods the deployment creates.

The key point is:

the selector and the pod-template labels must line up.

In this lesson they line up through:

```yaml
component: web
```

### Container Block

The pod template contains one container:

- `name: client`
- `image: stephengrider/multi-client`
- `containerPort: 3000`

So this is the Kubernetes form of the browser-facing frontend.

## Why This Lesson Matters

This is the first point where the migration becomes concrete.

The app is no longer just "something we will someday move to Kubernetes."

It now has:

- a dedicated manifest directory
- a first real deployment file
- a deliberate removal of older deployment infrastructure

That makes this lesson a real boundary between the old world and the new one.

## Important Caveat

The pasted YAML in the transcript looks slightly malformed in places, especially around indentation and `label` versus `labels`.

The durable structure is still clear from the spoken explanation and from the rest of the course:

- deployment
- three replicas
- `component: web`
- one `client` container
- image `stephengrider/multi-client`
- port `3000`

So the notes preserve the normalized version rather than copying the malformed paste literally.

## What Comes Next

The lesson ends by pointing to the next file:

the service for this deployment.

That is where the course plans to start explaining `ClusterIP` more directly.

## Best Reuse

Use this note when you want a simple explanation of:

- why the old project files are being deleted
- why the `k8s/` folder matters
- what the first real frontend deployment manifest contains
- why `selector.matchLabels` and pod-template labels must line up
