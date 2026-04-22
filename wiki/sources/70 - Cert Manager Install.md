---
title: Cert Manager Install
type: source
status: active
created: 2026-04-23
updated: 2026-04-23
source_path: raw/transcripts/70 - Cert Manager Install.md
source_date: 2026-04-22
tags:
  - kubernetes
  - cert-manager
  - helm
  - lets-encrypt
  - gke
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/67 - Helm and its setup.md
  - wiki/sources/71 - How to wire up Cert Manager.md
  - wiki/analysis/kubernetes-https-cert-manager-flow.md
---

# Cert Manager Install

## Metadata

- Source type: transcript plus a required update note with current Helm commands
- Transcript quality: strong enough to preserve the role of cert-manager and the modern install path
- Embedded visuals:
  - No local screenshots are embedded in this source
- Derived notes:
  - `derived/notes/70 - Cert Manager Install - Cheat Sheet.md`
  - `derived/notes/70 - Cert Manager Install - Explainer.md`

## Summary

This source installs cert-manager into the cluster so certificate issuance can be automated. The operationally important part is the update note: the modern path is Helm-based, run from Cloud Shell, and includes `installCRDs=true`. The transcript itself mainly frames cert-manager as the project that will handle the communication and challenge setup needed to obtain certificates from Let's Encrypt.

## Key Claims

- Cert-manager is the project the course uses to automate certificate issuance.
- The original video commands are outdated enough that the update note should be treated as the source of truth.
- The install now happens with Helm, not by blindly reusing the older video command.
- The Helm install should be run from GCP Cloud Shell against the live cluster.
- Installing cert-manager adds multiple Kubernetes objects such as deployments, pods, roles, and service accounts.

## Important Evidence Or Examples

The required update note gives the current install sequence:

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.8.0 \
  --set installCRDs=true
```

It also links the official install docs:

- https://cert-manager.io/docs/installation/helm/#steps

## Important Commands

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.8.0 \
  --set installCRDs=true
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source installs cert-manager but does not yet explain the issuer and certificate objects that make it useful.
- The update note pins a specific chart version, so future maintenance may need another modernization pass.
