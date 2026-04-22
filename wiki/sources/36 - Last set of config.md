---
title: Last set of config
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/36 - Last set of config.md
source_date: 2026-04-19
tags:
  - kubernetes
  - postgres
  - deployment
  - service
  - clusterip
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/35 - Volumes vs Persistent Volumes.md
---

# Last set of config

## Metadata

- Source type: transcript with embedded YAML and command-line walkthrough
- Transcript quality: strong enough to preserve the Postgres deployment and Postgres `ClusterIP` service manifests, plus the apply-and-verify flow
- Embedded visuals:
  - None explicitly provided in the transcript or newly added assets
- Derived notes:
  - `derived/notes/36 - Last set of config - Cheat Sheet.md`
  - `derived/notes/36 - Last set of config - Explainer.md`

## Summary

This source adds the last pair of repetitive infrastructure manifests: the Postgres deployment and the Postgres `ClusterIP` service. It uses the same structural pattern as the Redis setup, keeps Postgres at one replica, uses port `5432`, and ends by reapplying the full `k8s/` directory and checking that the Postgres pod and service are present. The lesson also explicitly says the more interesting part is still ahead: environment variables and the Postgres PVC.

## Key Claims

- Postgres should be deployed as a single-replica deployment for this course.
- The course is not attempting any clustered or replicated Postgres setup.
- The Postgres deployment should use the public `postgres` image.
- Postgres uses port `5432`.
- The matching service should be a `ClusterIP` service.
- The Postgres service selector should use `component: postgres`.
- The Postgres service should expose `port: 5432` and forward to `targetPort: 5432`.
- The server and worker will still need environment variables later in order to connect to Postgres.
- The Postgres PVC discussion is intentionally deferred until after the basic deployment and service are in place.

## Important Evidence Or Examples

- The intended Postgres deployment manifest is:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: postgres
  template:
    metadata:
      labels:
        component: postgres
    spec:
      containers:
        - name: postgres
          image: postgres
          ports:
            - containerPort: 5432
```

- The intended Postgres service manifest is:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: postgres-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: postgres
  ports:
    - port: 5432
      targetPort: 5432
```

- The apply-and-check workflow is:

```bash
kubectl apply -f k8s
kubectl get pods
kubectl get services
```

- The transcript explicitly frames this as the last batch of repetitive deployment-plus-service YAML before moving on to environment variables and persistence.
- The raw embedded YAML contains transcription problems such as `matchingLabels`, inconsistent capitalization in the service name, and dropped indentation around `template`, so the normalized manifests above follow the spoken walkthrough rather than the broken pasted block.

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

- The deployment and service are structurally in place, but the app still cannot use Postgres until the environment variables are wired into the server and worker.
- The lesson repeatedly points toward a Postgres PVC, but the actual persistence mechanism is still deferred to the next storage-focused sources.
- The raw transcript YAML is less reliable than the spoken explanation, so copied manifests need normalization before use.
