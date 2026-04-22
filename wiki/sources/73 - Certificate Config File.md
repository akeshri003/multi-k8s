---
title: Certificate Config File
type: source
status: active
created: 2026-04-23
updated: 2026-04-23
source_path: raw/transcripts/73 - Certificate Config File.md
source_date: 2026-04-22
tags:
  - kubernetes
  - cert-manager
  - certificate
  - tls
  - secret
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/71 - How to wire up Cert Manager.md
  - wiki/sources/72 - Issuer Config File.md
  - wiki/sources/75 - Ingress for Config.md
  - wiki/analysis/kubernetes-https-cert-manager-flow.md
---

# Certificate Config File

## Metadata

- Source type: transcript plus required update note with corrected manifest details
- Transcript quality: strong enough to preserve the purpose of the certificate object and the updated schema changes
- Embedded visuals:
  - ![[raw/assets/Pasted image 20260423004252.png]]
  - ![[raw/assets/Pasted image 20260423004503.png]]
- Derived notes:
  - `derived/notes/73 - Certificate Config File - Cheat Sheet.md`
  - `derived/notes/73 - Certificate Config File - Explainer.md`

## Summary

This source creates the certificate manifest that tells cert-manager what certificate to obtain. The modern version uses `cert-manager.io/v1`, references the `letsencrypt-prod` issuer, names the secret that should store the resulting TLS material, and lists the domain names that the certificate should cover. The older transcript's ACME challenge block is no longer part of the modern certificate spec and should be removed.

## Key Claims

- The certificate object describes the certificate wanted for the domain.
- The object should store the resulting certificate material in a Kubernetes secret.
- The certificate references the issuer that should be used to obtain it.
- `commonName` identifies the primary domain.
- `dnsNames` lists the domain names the certificate should be valid for, such as root and `www`.
- On a modern cluster, the certificate manifest should use `apiVersion: cert-manager.io/v1`.
- The old ACME challenge config inside the certificate spec should be removed in the modern manifest.

## Important Config

```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: yourdomain-com-tls
spec:
  secretName: yourdomain-com
  issuerRef:
    name: letsencrypt-prod
    kind: ClusterIssuer
  commonName: yourdomain.com
  dnsNames:
    - yourdomain.com
    - www.yourdomain.com
```

## Important Evidence Or Examples

- The transcript uses `k8smulti.com` as the example domain and treats the `www` variant as a second DNS name.
- The update note links the cert-manager HTTP validation tutorial and a QA thread with troubleshooting context.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript's older ACME config block is now historical explanation rather than current operational guidance.
- This lesson defines the certificate object, but the HTTPS path still is not complete until ingress is updated to use the resulting secret.
