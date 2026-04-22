# Creating Ingress Configuration Cheat Sheet

## Source

- Transcript: `raw/transcripts/57 - Creating Ingress Configuration.md`
- Wiki source page: [[57 - Creating Ingress Configuration]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 1.19.50 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 1.22.45 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 1.23.40 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 1.28.50 PM.png]]

## Core Idea

The app now gets one ingress config that:

- sends `/` to the client service
- sends `/api/` to the server service
- removes `/api` before the request reaches the server

## Main YAML

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

## Apply Step

```bash
kubectl apply -f k8s
```

## Retrieval Cues

- "`ingress-service.yaml`"
- "`/` -> client"
- "`/api/` -> server"
- "rewrite `/api` away"
- "route to service names, not IPs"

## Caveat

The transcript and screenshot header are inconsistent.

The lesson clearly intends an ingress object, but one pasted block still shows:

```yaml
apiVersion: v1
kind: Service
```

So keep the routing idea, but do not trust the raw header blindly.

## Modern Equivalent

For the current cluster in this vault, the working modern shape is:

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

See also: [[kubernetes-modern-ingress-format]]
