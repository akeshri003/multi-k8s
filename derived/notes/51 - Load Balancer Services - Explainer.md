# Load Balancer Services Explainer

## Source

- Transcript: `raw/transcripts/51 - Load Balancer Services.md`
- Wiki source page: [[51 - Load Balancer Services]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 12.29.09 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.30.29 PM.png]]

## What This Lesson Is Doing

The course is almost done with the basic local Kubernetes setup.

At this point, it already has:

- deployments
- internal services
- PVC
- secrets
- env vars

The remaining networking question is:

how does outside traffic get into the app?

This lesson answers that by quickly revisiting `LoadBalancer` services before moving on to `Ingress`.

## What A `LoadBalancer` Service Does

The course says a `LoadBalancer` service really does two things:

1. it exposes one set of pods inside the cluster
2. it causes the cloud provider to create an external load balancer resource

So this is not just an internal Kubernetes object.

It also has a cloud-provider side effect.

## Why It Is Not A Good Fit Here

The big limitation in this lesson is simple:

a single load-balancer service is basically tied to one selected pod set.

But this app needs more than one outside-facing destination.

The course specifically frames the problem as needing to reach more than just one target like the client or the server alone.

That is why a single `LoadBalancer` service is too blunt an instrument here.

## Why The Course Moves To `Ingress`

`Ingress` is introduced as the newer and more capable answer for this routing problem.

The key difference is that `Ingress` is meant to route incoming traffic across services.

So instead of saying:

"Expose this one target"

the system can say:

"Take incoming traffic and send it to the right service."

That is a much better fit for a multi-part app.

## The Mental Model

Reduce the lesson to this:

- `LoadBalancer` = expose one selected service path
- `Ingress` = route incoming traffic across multiple service destinations

That is the real transition.

## Best Reuse

Use this note when you want to remember:

- what a `LoadBalancer` service is doing at a high level
- why it is not the chosen approach for this app
- why `Ingress` is the next networking object in the sequence
