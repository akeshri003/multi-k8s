# The result of ingress-nginx Explainer

## Source

- Transcript: `raw/transcripts/68 - The result of ingress-nginx.md`
- Wiki source page: [[68 - The result of ingress-nginx]]

## Key Visual

- ![[raw/assets/Screenshot 2026-04-22 at 8.44.22 PM.png]]

## What This Lesson Confirms

The previous Helm lesson installed software into the cluster.

This lesson shows the result of that install in a form you can actually inspect:

- controller workload exists
- default backend exists
- external IP exists

So this is the lesson where ingress-nginx stops being an install command and becomes visible infrastructure.

## Why The Default Backend Matters

The default backend is the catch-all fallback.

Before your own ingress rules are active, requests reaching the controller will typically hit this fallback and return a default 404 response.

That is not a failure of ingress-nginx.

It is the expected state before your app routing is fully configured.

## How To Think About The Load Balancer Chain

The useful mental model here is:

1. Google Cloud provisions a real cloud load balancer.
2. That load balancer targets the cluster nodes.
3. Inside the cluster, traffic lands on the ingress-nginx `LoadBalancer` service.
4. That service feeds the ingress controller.
5. The controller uses your ingress config to route traffic to app services.

So ingress on GKE is not just one Kubernetes object.

It is Kubernetes objects plus cloud infrastructure working together.

## What This Means Operationally

After this point, the missing work is no longer "install ingress-nginx."

The missing work becomes:

- apply your app ingress config
- later add HTTPS wiring and certificates

That is why this lesson sits naturally between the Helm install and the cert-manager / HTTPS sequence.
