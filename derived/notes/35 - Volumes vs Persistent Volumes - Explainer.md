# Volumes vs Persistent Volumes Explainer

## Source

- Transcript: `raw/transcripts/35 - Volumes vs Persistent Volumes.md`
- Wiki source page: [[35 - Volumes vs Persistent Volumes]]

## What This Lesson Is Actually Doing

The title suggests this lesson will explain storage concepts:

- volumes
- persistent volumes

But the actual body content is still focused on something earlier in the architecture:

- creating the Redis deployment
- creating the Redis internal service

So the first thing to understand is:

this lesson is really a Redis config lesson, not yet a storage-concepts lesson.

## The Redis Deployment

The lesson builds this deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: redis
  template:
    metadata:
      labels:
        component: redis
    spec:
      containers:
        - name: redis
          image: redis
          ports:
            - containerPort: 6379
```

This follows the same deployment structure already used for:

- client
- server
- worker

But Redis is treated a little differently in one practical way:

the course keeps only one replica.

The transcript explicitly says Redis clustering exists, but this course is about Kubernetes, not about advanced Redis cluster setup.

So the course stays with one standalone Redis instance.

## The Redis Service

The lesson then builds the matching internal service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: redis
  ports:
    - port: 6379
      targetPort: 6379
```

This means:

- other objects inside the cluster can reach Redis
- Redis is not directly exposed to the outside world
- the service stays internal through `ClusterIP`

That matches the same architectural direction already used for the backend API.

## Why Port `6379`

Redis uses `6379` by default.

The transcript explicitly says there is no good reason to remap it here.

So the service keeps:

- `port: 6379`
- `targetPort: 6379`

That keeps the setup simple and consistent.

## Why This Lesson Matters

This lesson matters because the app is finally getting one of its missing shared dependencies.

Before this point:

- server existed
- worker existed
- but Redis was still missing

So even though this lesson is repetitive, it closes an important structural gap in the overall architecture.

It means the system is getting closer to:

- frontend
- backend
- worker
- Redis

all existing as Kubernetes objects inside the same cluster.

## Apply And Verify

The lesson ends by reapplying the whole `k8s/` directory and checking:

```bash
kubectl apply -f k8s
kubectl get pods
kubectl get services
```

That matches the batch-apply workflow the course has already established.

## Important Transcript Problems

This lesson has two notable source-quality issues.

First:

the title says `Volumes vs Persistent Volumes`, but the lesson body does not actually teach that topic yet.

Second:

the embedded YAML block at the top has obvious transcription problems, including `kind: Service` where the spoken walkthrough is clearly describing a deployment.

So the safest way to use this lesson is:

- trust the spoken explanation
- normalize the YAML structure
- record the title/body mismatch explicitly

## What Is Still Missing

Even after this lesson:

- the app still needs the Postgres deployment and service
- the backend and worker still need their environment variables wired correctly
- the actual promised storage discussion still has not happened yet

So Redis is an important step, but not the end of the data-layer setup.

## Best Reuse

Use this note when you want to remember:

- how Redis is deployed in the course
- why Redis gets a `ClusterIP` service
- why the course keeps only one Redis replica
- why this lesson's title should not be taken literally yet
