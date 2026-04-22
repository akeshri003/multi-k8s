---
title: Kubernetes Modern Ingress Format
type: analysis
status: active
created: 2026-04-20
updated: 2026-04-20
tags:
  - kubernetes
  - ingress
  - ingress-nginx
  - api-migration
  - deprecation
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/56 - Ingress Nginx installation and setup.md
  - wiki/sources/57 - Creating Ingress Configuration.md
---

# Kubernetes Modern Ingress Format

## Question

The course uses an older ingress config format. Why does that old manifest fail on a modern cluster, and what does the modern ingress config format look like?

## Short Answer

The old course manifest fails because it mixes multiple outdated patterns:

- older API versions such as `extensions/v1beta1`
- older backend fields such as `serviceName` and `servicePort`
- older ingress-class wiring through `kubernetes.io/ingress.class`

On a modern cluster, the stable ingress API is:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
```

and the backend shape is now nested under `service.name` and `service.port.number`.

## Why The Old Course Format Breaks

According to the Kubernetes deprecated API migration guide, the `extensions/v1beta1` and `networking.k8s.io/v1beta1` versions of `Ingress` are no longer served as of Kubernetes `v1.22`, and manifests should migrate to `networking.k8s.io/v1`.

The migration guide also calls out several schema changes:

- backend `serviceName` becomes `service.name`
- numeric backend `servicePort` becomes `service.port.number`
- string backend `servicePort` becomes `service.port.name`
- `pathType` is required on each path

That matches the errors seen in this vault:

- `kind: Service` with `spec.rules` fails because `rules` is an ingress field, not a service field
- `kind: Ingress` with `apiVersion: v1` fails because `Ingress` does not live in plain core `v1`
- `backend.serviceName` and `backend.servicePort` fail because that is the older ingress backend shape

## Ingress Class: Old Annotation vs Modern Field

The Kubernetes ingress docs explain that before `IngressClass` and `ingressClassName` were added, many controllers used:

```yaml
kubernetes.io/ingress.class: nginx
```

That annotation is now deprecated in favor of:

```yaml
spec:
  ingressClassName: nginx
```

For new manifests, the modern field is the preferred choice.

In this vault's current cluster behavior, using both the old annotation and the new field at the same time caused validation failure, so the modern manifest should keep `ingressClassName` and drop the old class annotation.

## Modern Format For This Vault

For the current `ingress-nginx` setup in this vault, the modern manifest shape is:

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

## How To Read The Modern Shape

### Header

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
```

This tells Kubernetes to use the stable ingress API.

### Metadata

```yaml
metadata:
  name: ingress-service
```

This gives the ingress object its identity.

### Controller-Specific Annotations

```yaml
annotations:
  nginx.ingress.kubernetes.io/use-regex: "true"
  nginx.ingress.kubernetes.io/rewrite-target: /$1
```

These are not generic Kubernetes ingress fields.

They are nginx-ingress-specific behavior controls:

- `use-regex` allows regex-style path matching
- `rewrite-target` removes the public prefix before the request reaches the backend

### Ingress Class

```yaml
spec:
  ingressClassName: nginx
```

This tells the cluster which ingress controller should implement the object.

### Rules And Paths

```yaml
rules:
  - http:
      paths:
```

This is the actual routing table.

Each path says:

- what incoming URL pattern to match
- what service to send the request to

### Required `pathType`

```yaml
pathType: ImplementationSpecific
```

Modern ingress requires `pathType`.

The Kubernetes migration guide says `ImplementationSpecific` is the closest replacement when you want the older controller-defined path behavior.

### Modern Backend Shape

```yaml
backend:
  service:
    name: server-cluster-ip-service
    port:
      number: 5000
```

This is the new stable format.

Instead of flat `serviceName` and `servicePort` fields, the backend now points to a nested service object.

## One Important Boundary

The ingress resource itself is still valid and stable, but the Kubernetes ingress docs also note that the API is frozen and that newer features are going into Gateway API instead of Ingress.

So the right mental model is:

- old course ingress YAML is deprecated and needs migration
- modern `networking.k8s.io/v1` ingress is still valid
- Kubernetes is not removing ingress itself, but it is no longer where new routing features are being added

## Practical Takeaway

For this vault:

- keep the course lesson for the routing idea
- do not reuse the old ingress YAML header and backend fields on a modern cluster
- prefer `networking.k8s.io/v1`
- prefer `spec.ingressClassName`
- use `service.name`, `service.port.number`, and `pathType`
- keep nginx-specific rewrite behavior in annotations, because that part belongs to the ingress controller rather than the Kubernetes core schema

## Citations And References

- Kubernetes Deprecated API Migration Guide:
  https://kubernetes.io/docs/reference/using-api/deprecation-guide/
- Kubernetes Ingress documentation:
  https://kubernetes.io/docs/concepts/services-networking/ingress/
