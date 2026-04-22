---
title: Deploying Changes
type: source
status: active
created: 2026-04-23
updated: 2026-04-23
source_path: raw/transcripts/74 - Deploying Changes.md
source_date: 2026-04-22
tags:
  - kubernetes
  - cert-manager
  - deployment
  - ci-cd
  - ingress
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/72 - Issuer Config File.md
  - wiki/sources/73 - Certificate Config File.md
  - wiki/sources/75 - Ingress for Config.md
  - wiki/analysis/kubernetes-https-cert-manager-flow.md
---

# Deploying Changes

## Metadata

- Source type: transcript describing the deploy trigger for issuer and certificate creation
- Transcript quality: strong enough to preserve the sequencing dependency between deploying issuer/certificate and updating ingress for TLS
- Embedded visuals:
  - No local screenshots are embedded in this source
- Derived notes:
  - `derived/notes/74 - Deploying Changes - Cheat Sheet.md`
  - `derived/notes/74 - Deploying Changes - Explainer.md`

## Summary

This source explains the sequencing of the HTTPS rollout. First, the issuer and certificate manifests must be deployed so cert-manager can start the certificate request flow and create the TLS secret. Only after the certificate exists should the ingress manifest be updated to use that secret for HTTPS traffic. The transcript uses the older Travis flow, but the durable idea is the deploy order rather than the CI provider.

## Key Claims

- The ingress manifest should not be switched to HTTPS until the certificate has actually been obtained and stored in a secret.
- The issuer and certificate manifests need to be deployed first so cert-manager can see them.
- Once those objects exist in the cluster, cert-manager automatically begins the Let's Encrypt flow.
- After the certificate is issued and the secret exists, ingress can be updated to use that secret.
- The transcript's Git / Travis steps are historical delivery details rather than the durable infrastructure lesson.

## Important Evidence Or Examples

- The spoken sequence is:
  - create issuer
  - create certificate
  - deploy app
  - let cert-manager obtain the certificate
  - only then update ingress to use the new secret
- The lesson expects a wait of roughly 10 to 15 minutes for cert-manager to finish the flow.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source assumes the CI deploy path already works; it does not explain how to debug a failed certificate issuance.
- The newer repo in this vault uses GitHub Actions rather than Travis, so only the sequencing should be preserved directly.
