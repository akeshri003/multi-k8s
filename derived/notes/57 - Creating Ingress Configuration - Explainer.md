# Creating Ingress Configuration Explainer

## Source

- Transcript: `raw/transcripts/57 - Creating Ingress Configuration.md`
- Wiki source page: [[57 - Creating Ingress Configuration]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 1.19.50 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 1.22.45 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 1.23.40 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 1.28.50 PM.png]]

## What This Lesson Is Doing

This is the first lesson where ingress stops being just an idea and becomes a real config file.

The file is called `ingress-service.yaml`, and it becomes the public routing layer for the app.

## The Routing Rule

The lesson keeps the same routing split the course used earlier with custom nginx:

- if the request starts with `/api`, it should go to the server
- otherwise, it should go to the client

That is exactly what the first three diagrams are showing.

## Why The Rewrite Exists

The rewrite rule is the most important detail in this lesson:

```yaml
nginx.ingress.kubernetes.io/rewrite-target: /
```

The idea is simple:

- the browser may request something like `/api/values`
- ingress uses `/api/` to decide that this belongs to the server
- before the request reaches the server, ingress removes the `/api` prefix

So the server can keep simpler route definitions and does not need to be tightly coupled to the public URL prefix.

That is the same design idea the course used earlier in the Elastic Beanstalk custom nginx setup.

## The Actual Backends

The ingress does not point at pod IP addresses.

It points at service names:

- `client-cluster-ip-service`
- `server-cluster-ip-service`

That is an important transition in the course.

The public entry point is now becoming:

- ingress rule
- then service
- then pods

not:

- browser
- directly to pod IP

## The Readable Manifest

The intended manifest from the spoken walkthrough is:

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

## Important Source Caveat

The source is internally inconsistent.

The spoken explanation says this file is an ingress object and even names an older ingress API version, but the pasted header visible in one block still shows:

```yaml
apiVersion: v1
kind: Service
```

That header does not match the rest of the lesson.

So the safe reading is:

- the routing logic is the important durable part
- the raw pasted header is not fully trustworthy

## Modern Replacement For The Course YAML

The course idea is still fine, but the manifest shape needs to be updated on a modern cluster.

The most important changes are:

- use `networking.k8s.io/v1`
- keep `kind: Ingress`
- use `spec.ingressClassName` instead of the older class annotation
- add required `pathType`
- replace `backend.serviceName` and `backend.servicePort` with `backend.service.name` and `backend.service.port.number`

For this vault's current cluster, the modern equivalent is:

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

That preserves the same routing meaning, but expresses it in the stable schema your current cluster expects.

For the full migration reasoning, see [[kubernetes-modern-ingress-format]].

## Final Step In The Lesson

The lesson ends by applying the config:

```bash
kubectl apply -f k8s
```

and expecting to see `ingress-service created`.

So this source is the exact moment where the course moves from ingress theory into the first real ingress manifest.
