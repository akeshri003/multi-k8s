# Last set of config Explainer

## Source

- Transcript: `raw/transcripts/36 - Last set of config.md`
- Wiki source page: [[36 - Last set of config]]

## What This Lesson Is Doing

The course has already built:

- client deployment and service
- server deployment and service
- worker deployment
- Redis deployment and service

This lesson adds the remaining basic database pair:

- Postgres deployment
- Postgres internal service

So this is the final deployment-and-service repetition before the course moves into the more interesting wiring and persistence topics.

## The Postgres Deployment

The lesson builds this deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: postgres
  template:
    metadata:
      labels:
        component: postgres
    spec:
      containers:
        - name: postgres
          image: postgres
          ports:
            - containerPort: 5432
```

This follows the same general pattern already used for the other deployments.

The special case here is that the course again keeps the database at one replica.

The transcript explicitly says more advanced Postgres clustering exists, but that is outside the scope of the Kubernetes lesson sequence.

## The Postgres Service

The matching service is:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: postgres-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: postgres
  ports:
    - port: 5432
      targetPort: 5432
```

This gives internal access to the Postgres pod while keeping it inside the cluster.

So the pattern now mirrors Redis:

- one database deployment
- one internal `ClusterIP` service

## Why Port `5432`

The transcript says Postgres is using its default port:

`5432`

And there is no reason to remap it here.

So the service simply forwards `5432` to `5432`.

That keeps the config easy to reason about.

## What This Unlocks

After this lesson, the cluster now has the full basic object set for:

- frontend
- backend API
- worker
- Redis
- Postgres

So the app architecture is now mostly present as Kubernetes objects.

But "mostly present" is not the same as "fully wired."

The course makes that explicit.

## What Is Still Missing

Two major missing pieces are called out directly:

- environment variables
- the Postgres PVC

That means the deployment/service shell is in place, but the runtime dependency wiring and the persistence mechanism are still not finished.

So this lesson is structurally important, but it is not the endpoint.

It is the handoff into the storage and environment-variable lessons.

## Important Transcript Problems

The raw embedded YAML has a few obvious errors:

- `matchingLabels` instead of `matchLabels`
- odd capitalization in `postgres-cluster-ip-Service`
- dropped indentation around `template`

The spoken walkthrough is more trustworthy than the pasted block, so the durable note normalizes the manifest structure instead of copying the broken text literally.

## Why This Lesson Matters

This lesson closes the repetitive manifest-writing phase.

That matters because the rest of the course can finally move from:

- basic object creation

to:

- dependency wiring
- persistence
- more realistic application behavior

So even though this lesson feels repetitive, it is the last structural setup step before the more interesting Kubernetes-specific problems begin.

## Best Reuse

Use this note when you want to remember:

- what the Postgres deployment and service look like
- why Postgres stays at one replica here
- why the service is internal-only
- which important pieces are still missing after the Postgres manifests exist
