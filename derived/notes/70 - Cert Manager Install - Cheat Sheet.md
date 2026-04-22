# Cert Manager Install Cheat Sheet

## Source

- Transcript: `raw/transcripts/70 - Cert Manager Install.md`
- Wiki source page: [[70 - Cert Manager Install]]

## Current Install Commands

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.8.0 \
  --set installCRDs=true
```

## Durable Idea

- cert-manager is the cluster component that automates certificate issuance and renewal work

## Docs

- https://cert-manager.io/docs/installation/helm/#steps
