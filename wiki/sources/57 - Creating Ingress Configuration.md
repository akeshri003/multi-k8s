---
title: Creating Ingress Configuration
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/57 - Creating Ingress Configuration.md
source_date: 2026-04-20
tags:
  - kubernetes
  - ingress
  - ingress-nginx
  - routing
  - rewrite
  - clusterip
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/28 - The ClusterIP Config.md
  - wiki/sources/31 - ClusterIP for Express API.md
  - wiki/sources/56 - Ingress Nginx installation and setup.md
  - wiki/analysis/kubernetes-modern-ingress-format.md
---

# Creating Ingress Configuration

## Metadata

- Source type: transcript with embedded diagrams, pasted YAML, and a short apply workflow
- Transcript quality: strong enough to preserve the routing intent, the ingress annotations, the service-name-based backends, and the final `kubectl apply -f k8s` step
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-20 at 1.19.50 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 1.22.45 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 1.23.40 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-20 at 1.28.50 PM.png]]
- Derived notes:
  - `derived/notes/57 - Creating Ingress Configuration - Cheat Sheet.md`
  - `derived/notes/57 - Creating Ingress Configuration - Explainer.md`

## Summary

This source creates the first actual ingress manifest for the Fibonacci app. The ingress config becomes the public entry point for the cluster: traffic to `/` goes to the client service, traffic to `/api/` goes to the server service, and the nginx rewrite annotation strips `/api` before the request reaches the Express server. The lesson also makes a practical shift in how routing is described: ingress points at service names and ports, not raw pod IPs.

## Key Claims

- The new file is named `ingress-service.yaml` and lives in the `k8s/` directory.
- The lesson intends to create an `Ingress` object rather than another `Service`.
- The ingress metadata uses `kubernetes.io/ingress.class: nginx` to target the nginx ingress controller.
- The ingress also uses `nginx.ingress.kubernetes.io/rewrite-target: /` so that `/api/...` can be sent to the server without the leading `/api`.
- Requests for `/` should route to `client-cluster-ip-service` on port `3000`.
- Requests for `/api/` should route to `server-cluster-ip-service` on port `5000`.
- The ingress config refers to service names rather than hard-coded cluster IP addresses.
- After creating the file, the lesson applies the whole `k8s/` directory and expects to see `ingress-service created`.

## Important Evidence Or Examples

- The first three diagrams progressively show the routing idea:
  - inspect the incoming request path
  - send `/` traffic to the client
  - send `/api` traffic to the server
  - remove `/api` before the request reaches the server
- The final screenshot shows the actual `ingress-service.yaml` editor view, including the ingress annotations and the two backend service names.
- The lesson explicitly ties the rewrite behavior back to the earlier Elastic Beanstalk custom nginx setup.
- The transcript ends by applying the config with `kubectl apply -f k8s` and says the output should include `ingress-service created`.

## Important Commands

```bash
kubectl apply -f k8s
```

## Important Config

The transcript is internally inconsistent about the top of the YAML. The spoken walkthrough says this file is an ingress object with an older ingress API version, while the pasted block and one screenshot still show `apiVersion: v1` and `kind: Service`.

The intended manifest, based on the spoken walkthrough plus the rest of the YAML body, is:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-service
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - http:
        paths:
          - path: /
            backend:
              serviceName: client-cluster-ip-service
              servicePort: 3000
          - path: /api/
            backend:
              serviceName: server-cluster-ip-service
              servicePort: 5000
```

## Modern Equivalent

For the current cluster in this vault, the stable ingress API shape is different from the older course example:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-service
  annotations:
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  ingressClassName: nginx
  rules:
    - http:
        paths:
          - path: /api/?(.*)
            pathType: ImplementationSpecific
            backend:
              service:
                name: server-cluster-ip-service
                port:
                  number: 5000
          - path: /?(.*)
            pathType: ImplementationSpecific
            backend:
              service:
                name: client-cluster-ip-service
                port:
                  number: 3000
```

This keeps the same routing intention as the course:

- `/api` traffic goes to the server
- everything else goes to the client
- the public `/api` prefix is rewritten away before the request reaches the server

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source uses an older ingress API shape and naming style, so a modern Kubernetes cluster may require a newer manifest format even if the routing idea stays the same.
- The spoken walkthrough and the screenshot header do not fully agree: the lesson clearly intends an ingress object, but the pasted top lines still show `apiVersion: v1` and `kind: Service`.
- The lesson explains the routing intent well, but it still postpones the deeper discussion of why the `rules`, `http`, and `paths` structure is shaped this way.
- The durable migration note for this source now lives in [[kubernetes-modern-ingress-format]].
