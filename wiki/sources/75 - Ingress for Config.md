---
title: Ingress for Config
type: source
status: active
created: 2026-04-23
updated: 2026-04-23
source_path: raw/transcripts/75 - Ingress for Config.md
source_date: 2026-04-22
tags:
  - kubernetes
  - ingress
  - https
  - cert-manager
  - tls
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/57 - Creating Ingress Configuration.md
  - wiki/sources/73 - Certificate Config File.md
  - wiki/sources/74 - Deploying Changes.md
  - wiki/analysis/kubernetes-https-cert-manager-flow.md
---

# Ingress for Config

## Metadata

- Source type: transcript plus required update note with a modern annotation change
- Transcript quality: strong enough to preserve the HTTPS-related ingress edits and the reason the cert-manager annotation matters
- Embedded visuals:
  - ![[raw/assets/Pasted image 20260423005538.png]]
  - ![[raw/assets/Pasted image 20260423005600.png]]
  - ![[raw/assets/Pasted image 20260423005641.png]]
  - ![[raw/assets/Pasted image 20260423005810.png]]
  - ![[raw/assets/Pasted image 20260423005820.png]]
  - ![[raw/assets/Pasted image 20260423005906.png]]
  - ![[raw/assets/Pasted image 20260423010007.png]]
- Derived notes:
  - `derived/notes/75 - Ingress for Config - Cheat Sheet.md`
  - `derived/notes/75 - Ingress for Config - Explainer.md`

## Summary

This source updates the ingress manifest so it can serve HTTPS traffic with the certificate obtained by cert-manager. The ingress gets a cert-manager cluster-issuer annotation, an SSL redirect annotation, a `tls` block naming the hosts and the certificate secret, and host-specific routing rules. The update note also preserves an important modern fix: the issuer annotation key should be `cert-manager.io/cluster-issuer`, not the older `certmanager.k8s.io/cluster-issuer`.

## Key Claims

- If `kubectl get certificates` initially shows no resources, updating and deploying the ingress can trigger issuance.
- The ingress needs a cluster-issuer annotation naming `letsencrypt-prod`.
- The modern annotation key is `cert-manager.io/cluster-issuer`.
- The ingress should force HTTPS by setting `nginx.ingress.kubernetes.io/ssl-redirect: "true"`.
- The ingress `tls` block lists the HTTPS hosts and the secret that contains the certificate.
- The secret named in `tls.secretName` should match the `secretName` from the certificate object.
- Host-specific rules are required so both the bare domain and `www` variant route correctly.

## Important Evidence Or Examples

The update note gives the corrected annotation key:

```yaml
cert-manager.io/cluster-issuer: "letsencrypt-prod"
```

The transcript's main ingress additions are:

```yaml
metadata:
  annotations:
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  tls:
    - hosts:
        - yourdomain.com
        - www.yourdomain.com
      secretName: yourdomain-com
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source is still shaped by the older course ingress layout, so the exact manifest may need modern `networking.k8s.io/v1` syntax in the real repo.
- The lesson focuses on TLS additions and does not re-teach the earlier ingress schema migration problem.
