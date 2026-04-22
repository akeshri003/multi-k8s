# Adding Environment Variables to Config Explainer

## Source

- Transcript: `raw/transcripts/47 - Adding Environment Variables to Config.md`
- Wiki source page: [[47 - Adding Environment Variables to Config]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 11.04.40 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 11.07.32 AM.png]]

## What This Lesson Is Doing

The previous lesson only introduced the idea that some environment variables are:

- plain constants
- connection-oriented values

This lesson turns that idea into real deployment config.

It updates:

- `worker-deployment`
- `server-deployment`

## The Worker Change

The worker gets the smallest env block:

```yaml
env:
  - name: REDIS_HOST
    value: redis-cluster-ip-service
  - name: REDIS_PORT
    value: "6379"
```

This fits the architecture the course has been building.

The worker needs Redis, but in this step it does not yet need the full Postgres set.

The important point is that the worker is not using some random hostname.

It uses the Kubernetes service name:

`redis-cluster-ip-service`

That is how the app reaches Redis inside the cluster.

## The Server Change

The server gets a larger env block:

```yaml
env:
  - name: REDIS_HOST
    value: redis-cluster-ip-service
  - name: REDIS_PORT
    value: "6379"
  - name: PGUSER
    value: postgres
  - name: PGHOST
    value: postgres-cluster-ip-service
  - name: PGPORT
    value: "5432"
  - name: PGDATABASE
    value: postgres
```

This makes sense because the server needs both dependencies:

- Redis
- Postgres

And again, the service names are doing the important connection work:

- `redis-cluster-ip-service`
- `postgres-cluster-ip-service`

## Why Service Names Matter

This lesson makes one Kubernetes pattern very concrete:

the app does not connect to pods directly.

It connects through services.

So the environment variable values are not raw pod IPs and not local-machine shortcuts.

They are the stable in-cluster service names.

That is a big part of why the earlier `ClusterIP` service lessons mattered.

## Why `PGPASSWORD` Is Missing

The transcript is very explicit here:

`PGPASSWORD` is being left out on purpose.

The reason is simple:

the password should not be written directly into the deployment YAML as plain text.

So this lesson gets the ordinary env vars in place first, and the next lesson handles the sensitive one.

## The Clean Mental Model

This lesson is easiest to remember like this:

- worker gets Redis connection settings
- server gets Redis and Postgres connection settings
- password is held back for secret handling

That is the whole transition from "objects exist" to "containers know how to find those objects."

## Best Reuse

Use this note when you want to remember:

- what the first real `env` blocks look like in the course
- which variables go to worker versus server
- why the service names are the connection targets
- why `PGPASSWORD` is intentionally deferred
