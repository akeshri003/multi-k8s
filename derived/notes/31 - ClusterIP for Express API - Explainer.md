# ClusterIP for Express API Explainer

## Source

- Transcript: `raw/transcripts/31 - ClusterIP for Express API.md`
- Wiki source page: [[31 - ClusterIP for Express API]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 8.46.02 PM.png]]

## What This Lesson Is Doing

The last lesson built the backend deployment:

- `server-deployment`
- `component: server`
- `containerPort: 5000`

This lesson adds the matching internal service for that backend.

So together, the two backend pieces are now:

- a deployment that creates the backend pods
- a `ClusterIP` service that gives other cluster objects a stable way to reach those pods

## The Service Manifest

The lesson writes this config:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: server-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: server
  ports:
    - port: 5000
      targetPort: 5000
```

This is intentionally very close to the frontend `ClusterIP` service.

That is the main teaching strategy here:

learn one pattern, then reuse it for the next app component.

## Why The Selector Is `component: server`

The service needs to know which pods belong to the backend API.

That is why it uses:

```yaml
selector:
  component: server
```

This matches the label applied by the backend deployment.

So the same repeated Kubernetes relationship appears again:

- deployment creates pods
- pods carry labels
- service uses a selector to find those pods

That is the stable pattern the course is reinforcing.

## Why Both Ports Are `5000`

The transcript reminds you that the `multi-server` image listens on port `5000`.

So the service is configured like this:

- `port: 5000`
- `targetPort: 5000`

Meaning:

- another object in the cluster talks to the service on `5000`
- the service forwards traffic to port `5000` on the backend pods

The diagram in this lesson makes that flow explicit and easy to visualize.

Just like the frontend `ClusterIP` lesson, the transcript says the numbers could be different.

But in this case there is no good reason to introduce that extra mismatch.

So the service port and pod port are kept the same.

## Why This Is A `ClusterIP`

The big architectural point is still the same:

the Express API should not be directly exposed to the outside world.

So this is not a `NodePort`.

It is an internal-only `ClusterIP`.

That means the backend API is treated as an internal app component that other cluster objects can call.

Later, `Ingress` will handle the outside entry path.

So this lesson keeps reinforcing the production-shaped design:

- public entry at the edge
- internal services for app-to-app communication

## Why This Lesson Matters

This lesson is short, but it completes an important backend pair:

- backend deployment
- backend internal service

That means the vault now has the same structural pattern on both sides:

- frontend deployment plus frontend `ClusterIP`
- backend deployment plus backend `ClusterIP`

That is a real step toward the full multi-service architecture from the earlier diagram.

## Important Caveats

- The lesson gives the backend `ClusterIP` service, but it still does not show the `Ingress` object that will eventually route traffic through the system.
- The service is structurally correct, but the backend app still needs Redis and Postgres environment variables from the previous lesson’s deferred setup.
- The lesson explains the service config, but it still does not show how another pod will call `server-cluster-ip-service` by name in practice.

## Best Reuse

Use this note when you want to remember:

- what the backend `ClusterIP` service manifest looks like
- why the selector is `component: server`
- why both ports are `5000`
- how the backend service mirrors the frontend `ClusterIP` pattern
