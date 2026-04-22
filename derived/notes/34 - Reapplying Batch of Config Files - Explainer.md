# Reapplying Batch of Config Files Explainer

## Source

- Transcript: `raw/transcripts/34 - Reapplying Batch of Config Files.md`
- Wiki source page: [[34 - Reapplying Batch of Config Files]]

## What This Lesson Is Doing

The last few lessons created three new pieces of config:

- the server deployment
- the server internal service
- the worker deployment

This lesson now throws all of that back into Kubernetes with one batch apply and checks what happened.

So this is a validation lesson.

It is not mainly about new objects.

It is about:

- reapplying the whole config directory
- checking the resulting objects
- looking at logs to see whether "running" really means "healthy enough"

## The Reapply Step

The main command is:

```bash
kubectl apply -f k8s
```

The lesson again reinforces an important workflow habit:

at this point in the course, you can safely reapply the whole directory instead of trying to choose one file at a time.

That is because Kubernetes compares object identity and updates existing objects rather than blindly duplicating them.

The shown output is:

```text
service/client-cluster-ip-service unchanged
deployment.apps/client-deployment unchanged
service/server-cluster-ip-service created
deployment.apps/server-deployment created
deployment.apps/worker-deployment created
```

That is exactly the pattern you want to understand:

- old objects can show as `unchanged`
- new objects can show as `created`

## The Verification Step

The lesson then checks:

```bash
kubectl get pods
kubectl get deployments
kubectl get services
```

The transcript shows:

- frontend deployment still healthy
- server deployment created with three pods
- worker deployment created with one pod
- backend service visible on port `5000`

So structurally, the cluster looks good.

## Why Logs Still Matter

The most important part of the lesson is the next step:

```bash
kubectl logs server-deployment-56685c959c-99zpc
```

This is where the course checks the difference between:

- Kubernetes object state
- actual app behavior

The log shows:

```text
Listening
Error: connect ECONNREFUSED 127.0.0.1:5432
```

So the server process starts, but then fails to connect to a dependency.

That means:

- Kubernetes was able to create the pod
- the process did start
- but the app is still not fully wired correctly

That is a valuable operational lesson.

## Why This Failure Is Expected

The transcript says the backend and worker still do not have the environment variables needed to connect to Redis or Postgres.

So this failure is not surprising.

The point of the lesson is not "everything should work now."

The point is:

- the manifests are structurally valid
- the objects can be created
- now the next missing layer is dependency wiring

## One Important Transcript Mismatch

The spoken explanation says the refusal is coming from Redis.

But the actual log line uses port `5432`, which is much more commonly associated with Postgres.

So the safest reading is:

the backend is failing to connect to a missing dependency, and the precise spoken diagnosis in the transcript should not be trusted too literally.

## Why This Lesson Matters

This lesson teaches a very practical Kubernetes habit:

do not stop at `kubectl get`.

A pod can exist, and even be `Running`, while the application inside is still misconfigured.

So real validation usually means moving through layers:

- apply manifests
- inspect objects
- inspect logs

That is exactly the progression this lesson models.

## Important Caveats

- The transcript's spoken explanation of the failing dependency does not cleanly match the shown `5432` log line.
- The lesson proves structural success, not full app correctness.
- Redis and Postgres integration are still deferred to later lessons.

## Best Reuse

Use this note when you want to remember:

- why reapplying the whole config directory is okay here
- how to read `created` versus `unchanged`
- why `kubectl get` is not enough on its own
- why `kubectl logs` is the first real sanity check after a deploy
