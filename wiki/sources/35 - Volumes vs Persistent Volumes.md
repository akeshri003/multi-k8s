---
title: Volumes vs Persistent Volumes
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/35 - Volumes vs Persistent Volumes.md
source_date: 2026-04-19
tags:
  - kubernetes
  - redis
  - deployment
  - service
  - storage
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/34 - Reapplying Batch of Config Files.md
---

# Volumes vs Persistent Volumes

## Metadata

- Source type: transcript with embedded YAML, command-line walkthrough, and a title that does not match the body content yet
- Transcript quality: strong enough to preserve the Redis deployment and Redis `ClusterIP` service manifests, plus the apply-and-verify workflow
- Embedded visuals:
  - None in `raw/assets`; the transcript includes a remote lecture thumbnail URL, which is not treated as a canonical local asset
- Derived notes:
  - `derived/notes/35 - Volumes vs Persistent Volumes - Cheat Sheet.md`
  - `derived/notes/35 - Volumes vs Persistent Volumes - Explainer.md`

## Summary

Despite its title, this source is still about building the Redis deployment and Redis `ClusterIP` service rather than explaining volumes. It adds a single-replica `redis-deployment` that runs the public `redis` image on port `6379`, and then adds a matching `redis-cluster-ip-service` so the server and worker can eventually connect to Redis inside the cluster. The lesson ends by reapplying the full `k8s/` directory and verifying that both the Redis pod and the Redis service appear.

## Key Claims

- Redis should be deployed as a single-replica deployment for this course.
- The course is not attempting a multi-node Redis cluster setup.
- The Redis deployment should use the public `redis` image directly.
- Redis listens on port `6379`.
- The Redis service should be a `ClusterIP` service.
- The Redis service selector should use `component: redis`.
- The Redis service should expose `port: 6379` and forward to `targetPort: 6379`.
- Reapplying the whole `k8s/` directory remains the normal workflow after adding the Redis manifests.

## Important Evidence Or Examples

- The intended Redis deployment manifest is:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: redis
  template:
    metadata:
      labels:
        component: redis
    spec:
      containers:
        - name: redis
          image: redis
          ports:
            - containerPort: 6379
```

- The intended Redis service manifest is:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: redis
  ports:
    - port: 6379
      targetPort: 6379
```

- The apply-and-verify workflow is:

```bash
kubectl apply -f k8s
kubectl get pods
kubectl get services
```

- The lesson explicitly says Redis is left as one standalone copy and does not attempt Redis clustering.
- The transcript also explains why there is no reason to remap the Redis port here: using `6379` on both sides keeps the setup simple.
- The raw embedded YAML at the top of the transcript contains clear transcription problems, including `kind: Service` where the spoken walkthrough describes a deployment, so the normalized manifests above follow the spoken explanation rather than the broken pasted block.

## Important Commands

```bash
kubectl apply -f k8s
kubectl get pods
kubectl get services
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript title promises `Volumes vs Persistent Volumes`, but the body of the lesson is still focused on Redis deployment and service setup, so the actual storage discussion appears to have been deferred or mislabeled.
- The source creates Redis and its service, but it still does not show the environment variables the server and worker will use to connect to it.
- The lesson confirms Redis object creation, but the broader app still lacks the Postgres side of the data layer.
