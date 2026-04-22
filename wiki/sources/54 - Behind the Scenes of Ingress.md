---
title: Behind the Scenes of Ingress
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/54 - Behind the Scenes of Ingress.md
source_date: 2026-04-20
tags:
  - kubernetes
  - ingress
  - ingresscontroller
  - controller
  - nginx
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/52. - Quick Note on Ingress.md
  - wiki/sources/55 - More Behind the Scenes of Ingress.md
---

# Behind the Scenes of Ingress

## Metadata

- Source type: transcript with controller and routing diagrams
- Transcript quality: strong enough to preserve the controller mental model for ingress and the course's explanation of how ingress-nginx combines controller and routing work
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-20 at 12.39.03 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 12.40.08 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 12.41.13 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 12.44.15 PM.png]]
- Derived notes:
  - `derived/notes/54 - Behind the Scenes of Ingress - Cheat Sheet.md`
  - `derived/notes/54 - Behind the Scenes of Ingress - Explainer.md`

## Summary

This source explains ingress by relating it to the deployment-controller pattern the course has already used. A deployment controller watches desired state and makes the pod reality match it. In the same way, ingress starts from an ingress config that describes routing rules, and an ingress controller works to make those routing rules real inside the cluster. The lesson then adds a project-specific detail: in ingress-nginx, the controller and the actual traffic-routing implementation are effectively packaged together.

## Key Claims

- A deployment is a kind of controller in Kubernetes.
- Controllers work to make desired state real inside the cluster.
- Ingress follows the same broad pattern:
  - config describes desired routing rules
  - controller implements them
- An ingress config is a set of routing rules.
- An ingress controller watches those rules and creates the infrastructure needed to obey them.
- In ingress-nginx specifically, the controller and the traffic-routing implementation are closely combined.
- The practical result is a deployment/pod that both reads ingress config and handles routing traffic.

## Important Evidence Or Examples

- The first diagram recaps the controller idea with deployments:
  - desired state
  - controller
  - new state
- The next ingress diagram mirrors that pattern:
  - ingress routing rules as desired state
  - ingress controller as the enforcing object
  - nginx pod handling routing as the realized state
- A later diagram shows the more generic ingress architecture:
  - ingress config
  - ingress controller
  - something that accepts incoming traffic
  - traffic routed to services
- The ingress-nginx-specific diagram simplifies that into two main pieces:
  - ingress config
  - ingress controller plus traffic-routing implementation together

## Important Commands

- No shell command is central in this source.
- The lesson is conceptual and controller-focused.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source explains the controller model well, but it still does not show the actual ingress manifest fields.
- The lesson says ingress-nginx combines controller and routing implementation, but it still postpones the real installation and runtime checks.
- The exact local setup remains deferred even after the behind-the-scenes explanation.
