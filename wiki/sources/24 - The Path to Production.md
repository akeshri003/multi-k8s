---
title: The Path to Production
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/24 - The Path to Production.md
source_date: 2026-04-19
tags:
  - kubernetes
  - production
  - ingress
  - clusterip
  - pvc
  - architecture
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/20 - Updating Deployment Images.md
  - wiki/sources/23 - Imperatively Updating a Deployment's Image.md
---

# The Path to Production

## Metadata

- Source type: transcript with embedded architecture and process diagrams
- Transcript quality: strong enough to preserve the upcoming multi-service architecture, the new object types being introduced, and the overall production path
- Embedded visuals:
  - ![[raw/assets/Pasted image 20260419201828.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 8.18.14 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 6.24.15 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 6.29.03 PM.png]]
- Derived notes:
  - `derived/notes/24 - The Path to Production - Cheat Sheet.md`
  - `derived/notes/24 - The Path to Production - Explainer.md`

## Summary

This source marks the transition from isolated Kubernetes exercises into the full multi-container application. The course now plans to move the older Fibonacci app into a Kubernetes architecture made up of multiple deployments and services. The high-level design keeps `multi-client`, `multi-server`, `Redis`, and `Postgres` inside Kubernetes, introduces an `Ingress Service`, replaces the earlier developer-facing `NodePort` approach with `ClusterIP` services for internal routing, and adds a `Postgres PVC` for persistent storage. The lesson also lays out the production path: create config files for everything locally, test on `minikube`, build a GitHub/Travis deployment flow, and finally deploy to AWS or Google Cloud.

## Key Claims

- The course is now moving from the small `simplek8s` exercises into the full multi-container application.
- The local development architecture is initially imagined on a single node, even though the eventual cloud deployment can expand to multiple nodes.
- `multi-client` and `multi-server` will each be managed by deployments.
- `Redis` and `Postgres` will also now be run inside Kubernetes rather than delegated to outside cloud-managed services.
- `Ingress Service` is introduced as a new entry-point concept.
- `ClusterIP` is introduced as the internal service type attached to most of the app's services in this architecture.
- `Postgres PVC` is introduced as a new persistence concept that will be explained later.
- The project will require many config files, roughly ten to twelve, covering services, deployments, and storage.
- The planned delivery path is:
  - create all config files
  - test locally on `minikube`
  - create a GitHub/Travis build-and-deploy flow
  - deploy to a cloud provider such as AWS or Google Cloud

## Important Evidence Or Examples

- The architecture diagram shows:
  - an `Ingress Service` receiving traffic
  - `ClusterIP` services in front of `multi-client`, `multi-server`, `Redis`, and `Postgres`
  - deployments managing `multi-client`, `multi-server`, `multi-worker`, `Redis`, and `Postgres`
  - a `Postgres PVC` attached to the `Postgres` side of the system
- The transcript explicitly contrasts this with the older Elastic Beanstalk setup:
  - earlier, Redis and Postgres were delegated to external AWS-managed services
  - now, the course will run them inside Kubernetes instead
- The process diagram gives the concrete step sequence:
  - create config files for each service and deployment
  - test locally on `minikube`
  - create a GitHub/Travis flow to build images and deploy
  - deploy the app to a cloud provider

## Important Commands

- No concrete shell command is taught in this source.
- The source is architectural and process-oriented rather than command-driven.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson introduces `Ingress Service`, `ClusterIP`, and `Persistent Volume Claim` at a high level, but it postpones their actual mechanics.
- The source intentionally assumes a single-node local picture first, even though the real production deployment can spread across multiple nodes.
- Running `Redis` and `Postgres` inside Kubernetes is presented as the next step here, but the operational tradeoffs versus managed cloud services are not yet discussed.
