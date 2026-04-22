# Ingress for Config Cheat Sheet

## Source

- Transcript: `raw/transcripts/75 - Ingress for Config.md`
- Wiki source page: [[75 - Ingress for Config]]

## Key Visuals

- ![[raw/assets/Pasted image 20260423005538.png]]
- ![[raw/assets/Pasted image 20260423005600.png]]
- ![[raw/assets/Pasted image 20260423005641.png]]
- ![[raw/assets/Pasted image 20260423005810.png]]
- ![[raw/assets/Pasted image 20260423005820.png]]
- ![[raw/assets/Pasted image 20260423005906.png]]
- ![[raw/assets/Pasted image 20260423010007.png]]

## HTTPS Ingress Additions

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

## Durable Idea

- ingress needs both:
  - TLS host/secret wiring
  - issuer annotation so cert-manager and ingress line up
