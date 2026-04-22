# The Worker Deployment Explainer

## Source

- Transcript: `raw/transcripts/33 - The Worker Deployment.md`
- Wiki source page: [[33 - The Worker Deployment]]

## What This Lesson Is Doing

The course has already built:

- the frontend deployment and service
- the backend deployment and service

This lesson adds the worker deployment.

At first glance, it looks like just one more copy of the same deployment pattern.

But the important part of the lesson is actually the difference:

the worker does not need a service at all.

## The Deployment Manifest

The lesson writes this config:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: worker-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: worker
  template:
    metadata:
      labels:
        component: worker
    spec:
      containers:
        - name: worker
          image: stephengrider/multi-worker
```

This matches the same broad pattern used for the client and server:

- deployment name
- replica count
- selector
- pod template
- one container

So the lesson is still reinforcing the deployment template pattern.

## Why The Label Is `component: worker`

The deployment still needs a selector:

```yaml
selector:
  matchLabels:
    component: worker
```

And the pod template still needs matching labels:

```yaml
labels:
  component: worker
```

So even though the worker has no service, it still follows the same deployment identity pattern as the other workloads.

## Why `replicas: 1` For Now

The transcript makes an interesting point here.

It says the worker is actually the part of the app most likely to need scaling later, because it performs the expensive Fibonacci calculation.

So in one sense, the worker is the best candidate for many replicas.

But the lesson still starts with:

```yaml
replicas: 1
```

That is a deliberate teaching choice.

It keeps the initial setup simple, and it leaves room for a later lesson to demonstrate scaling in a meaningful way.

## Why The Worker Needs No Service

This is the most important concept in the lesson.

The course says:

no other object in the cluster needs to make an unprompted request into the worker.

That means:

- no browser talks to the worker
- no ingress sends traffic directly to the worker
- no separate service needs to expose the worker

The worker is the thing that reaches outward to other parts of the app.

Because it is not an inbound traffic destination, it does not need:

- a service
- a port block

That is a useful Kubernetes design rule:

you only create a service when something needs a stable way to send requests into a pod or deployment.

## What Is Still Missing

The worker deployment is not truly complete yet.

The transcript explicitly says the worker will still need environment variables that tell it how to connect to Redis.

So the deployment shell is correct, but the runtime wiring is still deferred.

That means the lesson is structurally complete but operationally incomplete.

## Why This Lesson Matters

This lesson matters because it prevents a wrong pattern from solidifying.

After writing several deployments and services in a row, it would be easy to assume:

every deployment must have a service.

This lesson breaks that assumption.

It shows that services exist for inbound traffic paths, not just because an object happens to exist.

So the worker becomes the first clear example of:

- deployment without service

That is an important mental model upgrade.

## Important Caveats

- The raw pasted YAML in the transcript loses indentation around `template`, but the spoken walkthrough makes the intended structure clear.
- The worker still needs Redis environment variables later.
- The lesson says the worker is the best scaling candidate, but it does not yet show the workload signals that would drive that scaling decision in practice.

## Best Reuse

Use this note when you want to remember:

- why the worker gets a deployment
- why the worker does not get a service
- why there is no port block
- why the worker is likely to be the first component that needs scaling later
