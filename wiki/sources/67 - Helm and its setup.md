---
title: Helm and its setup
type: source
status: active
created: 2026-04-22
updated: 2026-04-22
source_path: raw/transcripts/67 - Helm and its setup.md
source_date: 2026-04-22
tags:
  - kubernetes
  - helm
  - ingress
  - ingress-nginx
  - gke
  - cloud-shell
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/56 - Ingress Nginx installation and setup.md
  - wiki/sources/57 - Creating Ingress Configuration.md
  - wiki/analysis/kubernetes-modern-ingress-format.md
---

# Helm and its setup

## Metadata

- Source type: transcript plus an important top-of-file update note with a modern Helm command
- Transcript quality: strong enough to preserve both the historical Helm-install explanation and the current ingress-nginx install correction
- Embedded visuals:
  - No local screenshots are embedded in this source
- Derived notes:
  - `derived/notes/67 - Helm and its setup - Cheat Sheet.md`
  - `derived/notes/67 - Helm and its setup - Explainer.md`

## Summary

This source handles the production-cluster side of ingress-nginx installation. The core idea is that the `Ingress` manifest written earlier is not enough by itself; the GKE cluster also needs the ingress-nginx controller installed as third-party software. The transcript introduces Helm as the tool for managing that installation, and the top update note replaces the older video commands with a current `helm upgrade --install ingress-nginx ...` command that should be run in Google Cloud Console.

## Key Claims

- A fresh production cluster does not know how to process the course's ingress config until ingress-nginx is installed.
- The earlier local `minikube addons enable ingress` step does not carry over to GKE.
- Helm is introduced as the tool for administering third-party Kubernetes software.
- The transcript says the course uses Helm here because a later step depends on it anyway.
- The updated note says the current ingress-nginx install command should be run through Helm in Google Cloud Console.
- The updated note also gives a concrete verification command for checking that the ingress controller service exists.

## Important Evidence Or Examples

The top-of-file update note gives the current production install command:

```bash
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

It also gives the verification step:

```bash
kubectl get service ingress-nginx-controller --namespace=ingress-nginx
```

The older transcript explains the Helm bootstrap conceptually:

- go to the Helm docs
- install Helm into Cloud Shell
- then use Helm instead of manually applying all ingress-nginx manifests

The updated note links directly to the ingress-nginx Helm deployment docs:

- https://kubernetes.github.io/ingress-nginx/deploy/#using-helm

## Important Commands

```bash
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace

kubectl get service ingress-nginx-controller --namespace=ingress-nginx
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source contains both a historical explanation and a modern corrected install command, so the update note should be treated as the operational source of truth.
- The transcript still speaks in general terms about installing Helm from docs rather than preserving the exact older install script inline.
- The next missing step is the actual first production deployment run after the secret and ingress-controller setup are finished.
