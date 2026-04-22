---
title: Issuer Config File
type: source
status: active
created: 2026-04-23
updated: 2026-04-23
source_path: raw/transcripts/72 - Issuer Config File.md
source_date: 2026-04-22
tags:
  - kubernetes
  - cert-manager
  - clusterissuer
  - lets-encrypt
  - acme
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/71 - How to wire up Cert Manager.md
  - wiki/sources/73 - Certificate Config File.md
  - wiki/analysis/kubernetes-https-cert-manager-flow.md
---

# Issuer Config File

## Metadata

- Source type: transcript plus required update note with a full corrected manifest
- Transcript quality: strong enough to preserve the purpose of the issuer and the updated modern schema
- Embedded visuals:
  - ![[raw/assets/Pasted image 20260423003502.png]]
- Derived notes:
  - `derived/notes/72 - Issuer Config File - Cheat Sheet.md`
  - `derived/notes/72 - Issuer Config File - Explainer.md`

## Summary

This source creates the issuer manifest used by cert-manager to talk to Let's Encrypt. The durable operational truth is in the update note: the modern object is `ClusterIssuer` in `cert-manager.io/v1`, and it needs an HTTP-01 solver that targets the nginx ingress class. The transcript's older `v1alpha1` material is still useful for understanding why the fields exist, but not as the final manifest to apply.

## Key Claims

- The issuer tells cert-manager where to go to request a certificate.
- The object kind is `ClusterIssuer`, not a built-in Kubernetes type.
- The issuer should target the Let's Encrypt production ACME directory.
- The email is a registration detail for Let's Encrypt.
- `privateKeySecretRef` is internal cert-manager / ACME state and is separate from the final site certificate secret.
- On a modern cluster, the issuer manifest should use `apiVersion: cert-manager.io/v1`.
- The modern manifest also needs a `solvers` block with an HTTP-01 nginx ingress solver.

## Important Config

```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: "test@test.com"
    privateKeySecretRef:
      name: letsencrypt-prod
    solvers:
      - http01:
          ingress:
            class: nginx
```

## Important Evidence Or Examples

- The transcript explains why issuer lives in the cert-manager API group rather than in core Kubernetes.
- The update note links the cert-manager issuer docs:
  - https://docs.cert-manager.io/en/latest/tasks/issuers/setup-acme/index.html#creating-a-basic-acme-issuer

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript's older `certmanager.k8s.io/v1alpha1` content should be treated as historical explanation only.
- The example email in the update note is a placeholder and should be replaced in real use.
