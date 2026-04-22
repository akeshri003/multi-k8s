---
title: Modern Local Kubernetes Development Setup
type: question
status: active
created: 2026-04-17
updated: 2026-04-17
tags:
  - kubernetes
  - local-development
  - tooling
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/2 - Kubernetes in Development and Production.md
---

# Modern Local Kubernetes Development Setup

## Why It Matters

The current source base introduces a useful distinction between local development and production, but the local setup guidance appears historically specific. The wiki needs a cleaner, current answer for how local Kubernetes development should usually be done.

## Current Partial Answer

- One source recommends `minikube` for local development and `kubectl` for cluster interaction ([[2 - Kubernetes in Development and Production]]).
- The same source also references `VirtualBox` as part of the setup flow, which suggests the instructions may be dated.
- One embedded screenshot points to Docker Desktop's built-in Kubernetes as an alternative, but that point is image-only context rather than a main transcript claim.

## Relevant Pages

- [[kubernetes]]
- [[2 - Kubernetes in Development and Production]]

## Next Sources Or Investigations

- Ingest a more recent Kubernetes local-setup source.
- Compare `minikube`, Docker Desktop Kubernetes, and other modern local options.
- Clarify which recommendations are platform-specific versus general.
