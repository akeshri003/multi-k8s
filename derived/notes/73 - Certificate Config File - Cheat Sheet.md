# Certificate Config File Cheat Sheet

## Source

- Transcript: `raw/transcripts/73 - Certificate Config File.md`
- Wiki source page: [[73 - Certificate Config File]]

## Key Visuals

- ![[raw/assets/Pasted image 20260423004252.png]]
- ![[raw/assets/Pasted image 20260423004503.png]]

## Modern Manifest

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

## Durable Idea

- certificate = what cert you want, which names it covers, and which secret should store it
