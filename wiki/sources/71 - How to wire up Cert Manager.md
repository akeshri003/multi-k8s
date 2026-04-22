---
title: How to wire up Cert Manager
type: source
status: active
created: 2026-04-23
updated: 2026-04-23
source_path: raw/transcripts/71 - How to wire up Cert Manager.md
source_date: 2026-04-22
tags:
  - kubernetes
  - cert-manager
  - lets-encrypt
  - certificate
  - issuer
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/69 - HTTPS SETUP OVERVIEW.md
  - wiki/sources/72 - Issuer Config File.md
  - wiki/sources/73 - Certificate Config File.md
  - wiki/analysis/kubernetes-https-cert-manager-flow.md
---

# How to wire up Cert Manager

## Metadata

- Source type: conceptual transcript with one architecture diagram
- Transcript quality: strong enough to preserve the roles of `ClusterIssuer` and `Certificate`
- Embedded visuals:
  - ![[raw/assets/Pasted image 20260423002153.png]]
- Derived notes:
  - `derived/notes/71 - How to wire up Cert Manager - Cheat Sheet.md`
  - `derived/notes/71 - How to wire up Cert Manager - Explainer.md`

## Summary

This source explains the two Kubernetes objects that connect cert-manager to the app. The issuer tells cert-manager which certificate authority to talk to and how to start the ACME flow. The certificate object describes the actual certificate wanted for the domain and the secret where the certificate material should be stored. Cert-manager watches for those objects and acts automatically when they appear.

## Key Claims

- Cert-manager itself is basically a pod plus supporting RBAC and service-account resources.
- The app must create two cert-manager-related objects:
  - an issuer
  - a certificate
- The issuer tells cert-manager which certificate authority to use and where that authority's API lives.
- Multiple issuers are possible, including staging and production issuers.
- The certificate object describes the domain names to include on the certificate.
- The certificate object also defines the Kubernetes secret that will hold the resulting certificate material.
- Cert-manager does not need those objects wired into it manually; it notices them and acts automatically.

## Important Evidence Or Examples

- The lesson prefers using Let's Encrypt production directly instead of first using staging with nginx ingress.
- The transcript explicitly separates:
  - issuer = where and how to ask
  - certificate = what cert to obtain and where to store it

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson explains the roles cleanly, but the next two lessons are still needed to capture the exact modern manifest structure.
- The source is conceptual and intentionally does not yet preserve the final working YAML.
