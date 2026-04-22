---
title: HTTPS SETUP OVERVIEW
type: source
status: active
created: 2026-04-23
updated: 2026-04-23
source_path: raw/transcripts/69 - HTTPS SETUP OVERVIEW.md
source_date: 2026-04-22
tags:
  - kubernetes
  - https
  - tls
  - cert-manager
  - lets-encrypt
  - dns
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/70 - Cert Manager Install.md
  - wiki/sources/71 - How to wire up Cert Manager.md
  - wiki/analysis/kubernetes-https-cert-manager-flow.md
---

# HTTPS SETUP OVERVIEW

## Metadata

- Source type: conceptual transcript with two diagrams
- Transcript quality: strong enough to preserve the certificate-validation handshake and the operational requirement that a real domain name is needed
- Embedded visuals:
  - ![[raw/assets/Pasted image 20260423001046.png]]
  - ![[raw/assets/Pasted image 20260423001125.png]]
- Derived notes:
  - `derived/notes/69 - HTTPS SETUP OVERVIEW - Cheat Sheet.md`
  - `derived/notes/69 - HTTPS SETUP OVERVIEW - Explainer.md`

## Summary

This source gives the high-level flow for adding HTTPS to the Kubernetes app. The cluster will ask Let's Encrypt for a certificate, Let's Encrypt will challenge the claimed domain by requesting a special path under `/.well-known/...`, and the cluster must answer that challenge correctly to prove domain ownership. The key operational constraint is that this only works if the user actually owns a real domain name and points it at the cluster.

## Key Claims

- HTTPS setup requires a purchased domain name.
- The course uses Let's Encrypt as the certificate authority.
- The cluster asks Let's Encrypt for a certificate for the chosen domain.
- Let's Encrypt verifies ownership by requesting a challenge URL under that domain.
- The cluster must respond correctly on that challenge route to prove it owns the domain.
- When validation succeeds, Let's Encrypt issues a certificate that is valid for about 90 days.
- The same process must repeat for renewal when the certificate nears expiration.
- The course does not make the user hand-code that challenge route; later infrastructure will automate it.

## Important Evidence Or Examples

- The source uses `multi-k8s.com` as the example domain.
- The challenge route is described as `/.well-known/<random-string>`.
- The transcript uses the "you could not fake google.com" comparison to explain why the challenge proves ownership.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson explains the certificate flow, but it deliberately postpones the exact Kubernetes objects that automate it.
- The source is about HTTP-01 style validation logic, not yet about the exact manifest format needed on a modern cluster.
