# Deployment Configuration files. Explainer

## Source

- Transcript: `raw/transcripts/15 - Deployment Configuration files..md`
- Wiki source page: [[15 - Deployment Configuration files.]]

## What This Lesson Changes

The previous lesson said deployments are the real object we should use for serious Kubernetes work.

This lesson makes that concrete by writing the first actual deployment YAML file.

So this is the moment where the course moves from:

- "deployments are probably better"

to:

- "here is the deployment manifest you should actually write"

## The Overall Shift

The source says something important very directly:

from this point forward, the course will stop creating pods manually and will instead create deployments that create pods.

That is the biggest takeaway.

Pods still matter, but they are no longer the main object you edit directly for application workloads.

## The Full Deployment Manifest

The lesson gives this deployment file:

![[raw/assets/Screenshot 2026-04-19 at 3.35.07 PM.png]]

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

This is the first real deployment configuration in the vault, so it is worth reading slowly.

## The Familiar Parts

Some of this should already look familiar from the earlier pod manifest lessons.

### `apiVersion` and `kind`

```yaml
apiVersion: apps/v1
kind: Deployment
```

This is the same basic pattern as before:

- choose the API version
- choose the object kind

The important new point is that `Deployment` lives in `apps/v1`, not plain `v1`.

### `metadata.name`

```yaml
metadata:
  name: client-deployment
```

This simply names the deployment object.

That part follows the same pattern as earlier Kubernetes manifests.

## The New Parts

The pieces that really matter in this lesson are all inside `spec`.

### `replicas`

```yaml
replicas: 1
```

This means the deployment should keep one pod running.

That is already a big difference from the earlier direct-pod model.

Before, you created one pod directly.

Now, you declare how many matching pods the deployment should maintain.

### `selector.matchLabels`

```yaml
selector:
  matchLabels:
    component: web
```

This is how the deployment knows which pods belong to it.

The lesson does not fully unpack it yet, but the basic idea is:

- the deployment looks for pods with matching labels
- those are the pods it manages

### `template`

```yaml
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

This is the pod template.

The easiest way to think about it is:

the deployment contains a built-in recipe for the pods it wants to create.

That recipe says:

- label the pod as `component: web`
- create a container named `client`
- use the image `stephengrider/multi-client`
- expose container port `3000`

So the deployment is not running the container directly.

It is defining the pod shape it wants, and then Kubernetes creates pods from that template.

## Why `multi-client` and Port `3000` Return Here

The lesson says the reason for using `multi-client` and exposing port `3000` is practical.

It wants a workload that can still be tested in the browser while switching from the older direct-pod approach to the new deployment-based approach.

So this is not just abstract YAML. It is a bridge from the previous examples into the new object model.

## The New Mental Model

After this lesson, the right simple model is:

1. write a deployment manifest
2. the deployment owns the desired pod count
3. the deployment uses a pod template
4. Kubernetes creates the actual pods from that template

That is the core transition.

## Best Reuse

Use this note when you want a simple explanation of:

- what the first deployment manifest looks like
- how a deployment differs from a direct pod
- what `replicas`, `selector`, and `template` are doing at a high level
- why the course is now moving away from direct pod creation

