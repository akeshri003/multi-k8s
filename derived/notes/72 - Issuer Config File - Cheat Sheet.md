# Issuer Config File Cheat Sheet

## Source

- Transcript: `raw/transcripts/72 - Issuer Config File.md`
- Wiki source page: [[72 - Issuer Config File]]

## Key Visual

- ![[raw/assets/Pasted image 20260423003502.png]]

## Modern Manifest

```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: "you@example.com"
    privateKeySecretRef:
      name: letsencrypt-prod
    solvers:
      - http01:
          ingress:
            class: nginx
```

## Durable Idea

- issuer = where cert-manager goes to start the ACME / Let's Encrypt flow
