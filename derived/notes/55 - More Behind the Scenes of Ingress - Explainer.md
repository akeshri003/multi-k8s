# More Behind the Scenes of Ingress Explainer

## Source

- Transcript: `raw/transcripts/55 - More Behind the Scenes of Ingress.md`
- Wiki source page: [[55 - More Behind the Scenes of Ingress]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 12.46.11 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.54.06 PM.png]]

## What This Lesson Is Doing

The previous lesson explained ingress in a generic controller-driven way.

This lesson now places that idea into the app’s actual architecture.

To make the picture concrete, it uses Google Cloud as the example environment.

## The Google Cloud Shape

The diagram shows several pieces working together:

- an ingress config
- an ingress-nginx deployment/pod
- a Google Cloud load balancer
- an in-cluster `LoadBalancer` service
- internal `ClusterIP` services
- a default-backend deployment

That is a much richer picture than just “Ingress routes traffic.”

## The Traffic Path

The traffic path here is:

1. outside traffic reaches the Google Cloud load balancer
2. that forwards traffic into the cluster’s load-balancer service
3. that traffic reaches the ingress-nginx pod
4. nginx routes it to the appropriate internal service

So ingress is still the high-level routing solution,
but the actual cloud-specific implementation involves more layers than the simple conceptual model suggested earlier.

## Why The `LoadBalancer` Service Reappears

This is the subtle but important part of the lesson.

Earlier, the course argued against using a `LoadBalancer` service as the main direct solution for the app.

That is still true at the application-design level.

But on Google Cloud, ingress-nginx still ends up involving a `LoadBalancer` service behind the scenes.

So the better mental model is:

- you are not manually designing your app around one raw load-balancer service
- but the ingress implementation may still use load-balancer infrastructure internally

That is not a contradiction once you separate:

- application design choice
- implementation details of the ingress system

## The Default Backend

The lesson also introduces a default-backend deployment.

Its stated role here is health checking.

The course says this is not the ideal final state.

Ideally, health checks would be more meaningfully tied to the actual application backend, but for the course flow the default backend remains in place.

## The Unfinished Comparison

The lesson ends by raising a natural question:

why not just build this yourself with:

- a load-balancer service
- a custom nginx deployment

The transcript starts to introduce that comparison, but it does not finish the argument in this source.

So the durable takeaway is:

the question matters, but the answer is incomplete here.

## Best Reuse

Use this note when you want to remember:

- what ingress-nginx looks like in a Google Cloud-style architecture
- why a `LoadBalancer` service can still appear behind the scenes
- what the default backend is doing
- why the simple “Ingress replaces load balancer” story needs a more nuanced interpretation
