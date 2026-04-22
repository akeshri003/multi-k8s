---
title: Load Balancer Services
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/51 - Load Balancer Services.md
source_date: 2026-04-20
tags:
  - kubernetes
  - service
  - loadbalancer
  - ingress
  - networking
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/27 - NodePort vs ClusterIP Services.md
  - wiki/sources/24 - The Path to Production.md
---

# Load Balancer Services

## Metadata

- Source type: transcript with architectural comparison diagrams
- Transcript quality: strong enough to preserve the role of `LoadBalancer` services, why the course considers them older than `Ingress`, and why they are not a good fit for this app
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-20 at 12.29.09 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 12.30.29 PM.png]]
- Derived notes:
  - `derived/notes/51 - Load Balancer Services - Cheat Sheet.md`
  - `derived/notes/51 - Load Balancer Services - Explainer.md`

## Summary

This source revisits Kubernetes service types right before the ingress setup begins. It gives a short overview of `LoadBalancer` services and explains why the course is not going to use them directly. The main reason is architectural: a single load-balancer service exposes one set of pods, but this application needs outside traffic to reach more than one destination, especially the client and server paths. That makes `Ingress` a better fit for the routing job ahead.

## Key Claims

- Kubernetes services are networking objects inside the cluster.
- `ClusterIP` exposes pods to other objects inside the cluster.
- `NodePort` exposes pods to the outside world, but mainly for development.
- `LoadBalancer` is described here as an older way of getting outside traffic into the cluster.
- A load-balancer service gives outside access to one specific set of pods.
- Creating a load-balancer service also causes Kubernetes to ask the cloud provider to create an external load balancer.
- A single load-balancer service is not a good fit for this app because the app needs traffic routed to more than one destination.
- The course prefers `Ingress` for this routing problem.

## Important Evidence Or Examples

- The first diagram compares the four networking options:
  - `ClusterIP`
  - `NodePort`
  - `LoadBalancer`
  - `Ingress`
- The diagram labels `LoadBalancer` as a legacy way of getting traffic into a cluster.
- The second diagram shows the `LoadBalancer` flow:
  - cloud-provider-managed external load balancer
  - Kubernetes `LoadBalancer` service
  - one selected set of pods, shown as the multi-server deployment
- The lesson explicitly says the app needs multiple outside-facing routes, not just a single target pod set.

## Important Commands

- No shell command is central in this source.
- The lesson is architectural rather than operational.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source strongly prefers `Ingress`, but it still leaves the exact ingress setup steps for later.
- The lesson calls `LoadBalancer` older or legacy, but it stops short of making a formal deprecation claim.
- The app needs multiple outside routes, but the exact path-based or host-based routing rules are still not shown here.
