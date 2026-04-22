---
title: Reapplying Batch of Config Files
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/34 - Reapplying Batch of Config Files.md
source_date: 2026-04-19
tags:
  - kubernetes
  - kubectl
  - deployment
  - logs
  - workflow
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/29 - Applying Multiple Files with Kubectl.md
  - wiki/sources/30 - Express API deployment config.md
  - wiki/sources/31 - ClusterIP for Express API.md
  - wiki/sources/33 - The Worker Deployment.md
---

# Reapplying Batch of Config Files

## Metadata

- Source type: transcript with command-line walkthrough and embedded terminal output
- Transcript quality: strong enough to preserve the batch re-apply workflow, the new pod/service state, and the first `kubectl logs` check against the backend pod
- Embedded visuals:
  - None explicitly provided in the transcript or newly added assets
- Derived notes:
  - `derived/notes/34 - Reapplying Batch of Config Files - Cheat Sheet.md`
  - `derived/notes/34 - Reapplying Batch of Config Files - Explainer.md`

## Summary

This source reapplies the full `k8s/` directory after the server and worker manifests have been added. It reinforces that reapplying the entire directory is safe for the current course stage because unchanged objects stay unchanged and new objects are created as needed. The lesson then uses `kubectl get` and `kubectl logs` to verify what happened. The main practical takeaway is that the server and worker deployments can appear healthy at the Kubernetes level even though the backend is still missing Redis and Postgres wiring.

## Key Claims

- At this stage of the course, reapplying the whole `k8s/` directory is a safe and normal workflow.
- Existing objects with the same type and name are not duplicated.
- New server and worker objects are created when first applied.
- `kubectl get pods`, `kubectl get deployments`, and `kubectl get services` are used to verify cluster state after the batch apply.
- `kubectl logs` is a quick way to inspect whether a new pod is actually behaving correctly.
- The new deployments may still fail functionally because Redis and Postgres are not wired in yet, even if Kubernetes reports them as running.

## Important Evidence Or Examples

- The batch apply workflow is:

```bash
kubectl apply -f k8s
```

- The apply output shown in the transcript is:

```text
service/client-cluster-ip-service unchanged
deployment.apps/client-deployment unchanged
service/server-cluster-ip-service created
deployment.apps/server-deployment created
deployment.apps/worker-deployment created
```

- The pod verification step is:

```bash
kubectl get pods
```

- The deployment verification step is:

```bash
kubectl get deployments
```

- The service verification step is:

```bash
kubectl get services
```

- The first log inspection step is:

```bash
kubectl logs server-deployment-56685c959c-99zpc
```

- The log output shown includes:

```text
> @ start /app
> node index.js
Listening
{ Error: connect ECONNREFUSED 127.0.0.1:5432
```

- The transcript treats this as expected partial failure because the backend still lacks the environment variables and service wiring it needs.

## Important Commands

```bash
kubectl apply -f k8s
kubectl get pods
kubectl get deployments
kubectl get services
kubectl logs <pod-name>
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript says the refusal is coming from an attempt to connect to Redis, but the shown port is `5432`, which is conventionally associated with Postgres, so the exact diagnosis in the spoken explanation is not fully reliable.
- The lesson shows the new deployments as `Running`, but it does not yet explain the difference between a container being alive and the application being fully functional.
- The source proves the batch apply workflow works, but it still leaves the actual Redis and Postgres integration for later lessons.
