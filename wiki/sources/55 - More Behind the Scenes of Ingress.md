---
title: More Behind the Scenes of Ingress
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/55 - More Behind the Scenes of Ingress.md
source_date: 2026-04-20
tags:
  - kubernetes
  - ingress
  - ingress-nginx
  - googlecloud
  - loadbalancer
  - default-backend
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/54 - Behind the Scenes of Ingress.md
  - wiki/sources/51 - Load Balancer Services.md
---

# More Behind the Scenes of Ingress

## Metadata

- Source type: transcript with cloud-specific ingress architecture diagrams
- Transcript quality: strong enough to preserve the Google Cloud-specific ingress-nginx architecture, the default-backend role, and the contrast with a custom nginx plus load-balancer setup
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-20 at 12.46.11 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 12.54.06 PM.png]]
- Derived notes:
  - `derived/notes/55 - More Behind the Scenes of Ingress - Cheat Sheet.md`
  - `derived/notes/55 - More Behind the Scenes of Ingress - Explainer.md`

## Summary

This source maps the ingress-nginx controller pattern onto the actual app architecture, specifically using Google Cloud as the reference environment. The ingress config feeds into an ingress-nginx deployment, Google Cloud creates an external load balancer, and a load-balancer service exists inside the cluster to feed traffic into the nginx controller pod. The lesson also introduces a default-backend deployment used for health checks and points out that, even though the course said it would not use a load-balancer service directly, a load-balancer service still appears behind the scenes in this Google Cloud ingress setup.

## Key Claims

- The exact ingress-nginx implementation differs by environment.
- The lesson uses Google Cloud as the easiest environment for explaining the full architecture.
- On Google Cloud, creating ingress ultimately leads to a cloud-native Google Cloud load balancer being created.
- Inside the cluster, a `LoadBalancer` service still appears as part of the ingress-nginx setup.
- The ingress-nginx deployment includes the controller plus an nginx pod that actually handles routing.
- Incoming traffic reaches the Google Cloud load balancer, then the in-cluster load-balancer service, then the nginx ingress pod, and finally the appropriate service.
- A default-backend deployment is also created and is used for health checks.
- The source explicitly raises, but does not yet fully answer, the question of why not just manually build a custom nginx plus load-balancer setup instead.

## Important Evidence Or Examples

- The main Google Cloud diagram shows:
  - Google Cloud Load Balancer
  - in-cluster LoadBalancer service
  - ingress config
  - nginx-controller/nginx pod deployment
  - client and server `ClusterIP` services
  - default-backend pod
- The lesson explicitly says the default backend is used for health checks.
- The final diagram contrasts the ingress-nginx setup with a hypothetical manual alternative:
  - cloud load balancer
  - manually managed nginx pod
  - cluster services behind it
- The source does not complete that comparison, so the “why not custom nginx?” question remains open at the end of the transcript.

## Important Commands

- No shell command is central in this source.
- The lesson is still conceptual and architecture-focused.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The lesson is environment-specific to Google Cloud for explanatory purposes, so the local setup may still look different in practice.
- The source acknowledges that a `LoadBalancer` service still appears behind the scenes, which complicates the earlier simpler story that ingress replaces load balancers.
- The transcript ends while introducing the “why not custom nginx?” comparison, so that argument remains incomplete in this source.
