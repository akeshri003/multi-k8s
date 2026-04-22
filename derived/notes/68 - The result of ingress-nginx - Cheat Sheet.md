# The result of ingress-nginx Cheat Sheet

## Source

- Transcript: `raw/transcripts/68 - The result of ingress-nginx.md`
- Wiki source page: [[68 - The result of ingress-nginx]]

## Key Visual

- ![[raw/assets/Screenshot 2026-04-22 at 8.44.22 PM.png]]

## What Appears After Install

- ingress controller workload
- default backend
- `LoadBalancer` service
- external IP

## Mental Model

- Google Cloud creates a cloud load balancer
- that feeds the Kubernetes `LoadBalancer` service
- that feeds the ingress-nginx controller
- that will eventually feed the app services

## What The External IP Shows Right Now

- `default backend - 404`

That is expected before the real app ingress rules are in place.

## Verify

```bash
kubectl get service ingress-nginx-controller --namespace=ingress-nginx
kubectl get pods --namespace=ingress-nginx
```
