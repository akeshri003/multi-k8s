---
title: Kubernetes in Development and Production
type: source
status: active
created: 2026-04-17
updated: 2026-04-17
source_path: raw/transcripts/2 - Kubernetes in Development and Production.md
source_date: 2026-04-17
tags:
  - kubernetes
  - minikube
  - kubectl
  - eks
  - gke
related:
  - wiki/topics/kubernetes.md
  - wiki/questions/modern-local-kubernetes-development-setup.md
---

# Kubernetes in Development and Production

## Metadata

- Source type: transcript with embedded screenshots
- Transcript quality: partial but coherent for the lesson's main distinctions
- Embedded visuals:
  - ![[raw/transcripts/Pasted image 20260417182435.png]]
  - ![[raw/transcripts/Screenshot 2026-04-17 at 6.28.52 PM.png]]

## Summary

This source distinguishes local Kubernetes development from production deployment. It argues that local development should use `minikube`, while production usually relies on managed Kubernetes services such as EKS or GKE, with `kubectl` acting as the common interface for cluster management across both environments.

## Key Claims

- Kubernetes usage differs substantially between local development and production.
- In local development, `minikube` creates a small Kubernetes cluster on the developer's machine.
- In production, teams often prefer managed services rather than building and operating a cluster entirely themselves.
- Amazon EKS and Google GKE are given as the primary managed examples.
- `kubectl` is the tool used to interact with and manage a Kubernetes cluster.
- `minikube` is local-only, whereas `kubectl` is used both locally and in production.

## Important Evidence Or Examples

- One diagram separates development from production and places `minikube` on the development side, while EKS, GKE, and do-it-yourself setups appear on the production side.
- A second diagram distinguishes `kubectl`, which manages workloads in the node, from `minikube`, which manages the local VM itself.
- The screenshot annotation that mentions Docker Desktop's built-in Kubernetes as an alternative appears to be image-only context rather than a claim stated in the transcript, so it should be treated as supporting evidence rather than the lesson's main thesis.
- The transcript recommends installing `kubectl`, a VM driver, `VirtualBox`, and then `minikube` for local setup. This is historically specific setup guidance and may not reflect the most current local Kubernetes workflow.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source treats `minikube` plus `VirtualBox` as the main local path, but one screenshot also points to Docker Desktop's Kubernetes as an alternative.
- The source is setup-oriented and likely dated in tooling details, even if the development-versus-production distinction remains useful.
- The wiki needs a clearer answer on what the modern recommended local Kubernetes setup should be.
