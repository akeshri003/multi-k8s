# Helm and its setup Cheat Sheet

## Source

- Transcript: `raw/transcripts/67 - Helm and its setup.md`
- Wiki source page: [[67 - Helm and its setup]]

## Why This Step Exists

- the ingress manifest alone is not enough
- the cluster also needs the ingress-nginx controller installed

## Current Recommended Command

```bash
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

## Verify

```bash
kubectl get service ingress-nginx-controller --namespace=ingress-nginx
```

## Durable Idea

- Helm is the package manager / installer for third-party Kubernetes software

## Docs

- https://kubernetes.github.io/ingress-nginx/deploy/#using-helm
