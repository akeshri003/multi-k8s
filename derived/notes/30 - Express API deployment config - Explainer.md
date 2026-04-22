# Express API deployment config Explainer

## Source

- Transcript: `raw/transcripts/30 - Express API deployment config.md`
- Wiki source page: [[30 - Express API deployment config]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 8.45.18 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 8.46.02 PM.png]]

## What This Lesson Is Doing

The frontend side of the app already has:

- a deployment
- a `ClusterIP` service

This lesson starts building the backend side of the same architecture.

It creates the deployment for the Express API, which uses the `multi-server` image.

So this lesson is basically the backend counterpart to the earlier `client-deployment` work.

## The Big Architectural Point

The transcript repeats an important rule:

the Express API should not be directly reachable from the outside world.

That is why the course says the API will also use a `ClusterIP` service instead of a `NodePort`.

The architecture diagram makes that placement clear:

- `Ingress` is at the edge
- `multi-client` sits behind one internal service
- `multi-server` sits behind another internal service

So the server is treated as an internal app component, not as a directly exposed public endpoint.

## The Deployment Manifest

The lesson writes this config:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: server-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      component: server
  template:
    metadata:
      labels:
        component: server
    spec:
      containers:
        - name: server
          image: stephengrider/multi-server
          ports:
            - containerPort: 5000
```

This is intentionally very similar to the frontend deployment.

That similarity is useful.

It means the course is building a repeated pattern:

- one deployment per major app component
- one label identity per component
- one pod template per deployment

## Why The Label Is `component: server`

The deployment needs a selector:

```yaml
selector:
  matchLabels:
    component: server
```

And the pod template needs matching labels:

```yaml
labels:
  component: server
```

That is how the deployment knows which pods belong to it.

The transcript also says this label could have been named differently.

So the important thing is not the exact wording.

The important thing is:

- pick a label scheme
- use it consistently
- make sure selector and template labels line up

## Why `containerPort: 5000`

The backend image listens on port `5000`.

The transcript says this is already hard-coded into the server process inside the image.

So the deployment is not inventing that port.

It is declaring the port the container already uses.

That is why the later service will also need to forward traffic to `5000`.

The second screenshot makes that flow concrete:

- another object in the cluster sends traffic to port `5000`
- the `ClusterIP` service receives that traffic
- the service forwards traffic to port `5000` on the selected server pods

## Why Three Replicas

The course again chooses `replicas: 3`.

The transcript is explicit that this is not based on deep capacity planning.

It is just a reasonable starting point for the lesson sequence.

That is useful because it shows the real meaning of `replicas`:

it is the desired number of identical backend pods Kubernetes should keep running.

## What Is Still Missing

This is the most important caveat in the lesson.

The deployment file is not actually finished yet.

The Express API needs environment variables so it can connect to:

- Postgres
- Redis

So at this stage, the lesson is only building the structural shell of the backend deployment:

- name
- labels
- image
- port
- replicas

But the runtime wiring to other services is still deferred.

That matters because it keeps the note honest:

the deployment shape is correct, but the full application wiring is not done yet.

## Why This Lesson Matters

This lesson is small, but it advances the migration in an important way.

The app is no longer just:

- frontend deployment
- frontend internal service

It is now becoming a multi-deployment architecture with separate internal components.

That is much closer to the production diagram introduced earlier.

This lesson also reinforces the larger Kubernetes habit:

once you understand one deployment pattern, you reuse it across different services by changing:

- names
- labels
- image
- ports

## Important Caveats

- The transcript mis-hears the filename as `server-deployment.yamo`; the intended file is `server-deployment.yaml`.
- The lesson talks about the matching backend `ClusterIP` service, but it does not build that service manifest yet.
- The deployment is still incomplete until Redis and Postgres environment variables are added.

## Best Reuse

Use this note when you want to remember:

- how the backend deployment mirrors the frontend deployment pattern
- why the Express API uses `component: server`
- why the container port is `5000`
- why the deployment is still incomplete without Redis/Postgres environment variables
