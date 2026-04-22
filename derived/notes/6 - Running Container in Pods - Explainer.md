# Running Container in Pods Explainer

## Source

- Transcript: `raw/transcripts/6 - Running Container in Pods.md`
- Wiki source page: [[6 - Running Container in Pods]]

## What This Lesson Fixes

Earlier sources introduced `Pod` as a Kubernetes object kind, but that still leaves an obvious question:

"Why do I need a Pod at all if I only want to run one container?"

This lesson answers that directly. In Kubernetes, you do not deploy a naked container by itself. The smallest thing Kubernetes deploys is a `Pod`.

## Basic Mental Model

The node is the machine. The pod is a deployable unit inside the node. The container runs inside the pod.

The first diagram in the lesson is the simple version:

![[raw/assets/Screenshot 2026-04-17 at 7.48.36 PM.png]]

That is the first useful picture to keep in your head:

- `node` is the machine created by minikube
- `pod` is the object living on that node
- the actual application container runs inside the pod

## Why Kubernetes Uses Pods

The lesson's core explanation is that pods exist to group containers that have an extremely tight operational relationship.

This does not mean:

- "containers that are part of the same broad app"

It means:

- containers that must be deployed together
- containers that are tightly integrated
- containers that become useless if the primary partner disappears

## The Postgres Example

The transcript uses a stronger example than the earlier multi-container app:

![[raw/assets/Screenshot 2026-04-17 at 8.43.26 PM.png]]

![[raw/assets/Screenshot 2026-04-17 at 8.45.37 PM.png]]

Here the lesson imagines:

- a primary `postgres` container
- a `logger` support container
- a `backup-manager` support container

This is a better pod example because the support containers are tightly coupled to the database. If the database disappears, the support containers are largely worthless.

## Why The Earlier App Should Not Just Be One Giant Pod

The lesson explicitly argues against treating the earlier multi-container app as "one pod with all four containers."

Reason:

- parts of that app could still continue to function in some degraded way if another container failed
- that is not the level of tight coupling that pods are meant to represent

So the useful distinction is:

- "related application parts" is not enough
- "must live and die together" is the better pod test

## How This Connects To The Existing YAML

The earlier manifest used:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: client-pod
  labels:
    component: web
spec:
  containers:
    - name: client
      image: stephengrider/multi-client
      ports:
        - containerPort: 3000
```

This lesson explains why that object is a `Pod` even though it currently contains only one container.

The answer is structural:

- Kubernetes deploys pods
- the pod is the unit
- the single `client` container is simply the first container inside that unit

## Best Reuse

Use this note when you need to remember:

- why Kubernetes deploys pods instead of naked containers
- when multiple containers belong together in one pod
- why tight coupling matters more than just "these containers are all part of my app"
