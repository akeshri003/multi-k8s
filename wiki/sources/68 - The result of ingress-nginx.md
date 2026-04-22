---
title: The result of ingress-nginx
type: source
status: active
created: 2026-04-23
updated: 2026-04-23
source_path: raw/transcripts/68 - The result of ingress-nginx.md
source_date: 2026-04-22
tags:
  - kubernetes
  - ingress
  - ingress-nginx
  - gke
  - load-balancer
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/67 - Helm and its setup.md
  - wiki/sources/75 - Ingress for Config.md
  - wiki/analysis/kubernetes-https-cert-manager-flow.md
---

# The result of ingress-nginx

## Metadata

- Source type: transcript with one cluster screenshot and a Google Cloud Console walkthrough
- Transcript quality: strong enough to preserve what ingress-nginx creates in GKE and how the cloud load balancer fits the picture
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-22 at 8.44.22 PM.png]]
- Derived notes:
  - `derived/notes/68 - The result of ingress-nginx - Cheat Sheet.md`
  - `derived/notes/68 - The result of ingress-nginx - Explainer.md`

## Summary

This source shows the first visible result of installing ingress-nginx on GKE. After Helm installation, the cluster now has an ingress controller workload, a default backend, and a `LoadBalancer` service with an external IP. The lesson also ties the Kubernetes objects back to Google Cloud infrastructure by showing that GKE created a Google Cloud load balancer in front of the cluster nodes.

## Key Claims

- Installing ingress-nginx creates an ingress controller deployment plus a default backend.
- The ingress controller is the thing that reads ingress config and programs nginx behavior.
- The default backend provides health checks and a fallback 404 response.
- The ingress-nginx service is of type `LoadBalancer`, which is why an external IP appears.
- The external IP shown on the ingress-nginx service is the address that will later expose the app.
- The Google Cloud Console load-balancer view reflects cloud infrastructure created to support ingress-nginx.
- The cloud load balancer points traffic at the cluster nodes, which then hand traffic to the in-cluster ingress-nginx service and controller.

## Important Evidence Or Examples

- The screenshot shows the controller workload present after the Helm install.
- The transcript says clicking the external IP on port `80` at this stage returns `default backend - 404`.
- The lesson explicitly maps the chain as:
  - Google Cloud load balancer
  - Kubernetes `LoadBalancer` service
  - ingress-nginx controller
  - application deployments

## Important Commands

```bash
kubectl get service ingress-nginx-controller --namespace=ingress-nginx
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson proves the ingress controller exists, but it still does not expose the app until the app ingress rules are applied.
- The source is about infrastructure visibility, not certificate issuance or HTTPS yet.
- The older placeholder source [[68 -]] should now be treated as incomplete earlier evidence rather than the main record for this lesson slot.
