---
title: Connecting to Running Containers
type: source
status: active
created: 2026-04-17
updated: 2026-04-17
source_path: raw/transcripts/8 - Connecting to Running Containers.md
source_date: 2026-04-17
tags:
  - kubernetes
  - kubectl
  - nodeport
  - networking
  - minikube
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/6 - Running Container in Pods.md
  - wiki/sources/7 - Service Config Files in Depth.md
---

# Connecting to Running Containers

## Metadata

- Source type: transcript with embedded command output, screenshots, and a short study-note preface
- Transcript quality: strong enough to preserve the operational flow for applying manifests, verifying object status, and accessing a NodePort-backed workload
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-17 at 10.48.55 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 10.53.07 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 10.55.35 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 10.58.19 PM.png]]
- Derived notes:
  - `derived/notes/8 - Connecting to Running Containers - Cheat Sheet.md`
  - `derived/notes/8 - Connecting to Running Containers - Explainer.md`

## Summary

This source turns the earlier pod-and-service manifests into a working workflow. It shows how to apply both manifests with `kubectl apply -f`, verify their status with `kubectl get`, and then reach the running application through the service's `NodePort`. It also makes the local-environment distinction explicit: in a `minikube` VM flow, the node is not `localhost`, while a Docker Desktop setup may allow `localhost` access instead.

## Key Claims

- `kubectl apply -f <file>` is the basic workflow for loading manifest files into the cluster.
- Fast "configured" or "created" output is not enough evidence by itself; `kubectl get` should be used to verify pod and service state.
- `kubectl get pods` provides a cluster-level status view including readiness, status, restarts, and age.
- `kubectl get services` shows the created services and reports the service `port` and `nodePort`, but not `targetPort`.
- In a `minikube`-style setup, a NodePort workload is accessed through the VM IP rather than `localhost`.
- The source's preface notes that Docker Desktop Kubernetes behaves differently and may allow `localhost` access, so the local access rule depends on the cluster runtime.
- A `Service` is required to access the running pod externally; the pod alone is not enough.

## Important Evidence Or Examples

- The source applies the two earlier manifests with:

```bash
kubectl apply -f client-pod.yaml
kubectl apply -f client-node-port.yaml
```

- The captured terminal output shows the objects being created:

```text
aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl apply -f client-pod.yaml

pod/client-pod created

aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl apply -f client-node-port.yaml

service/client-node-port created
```

- The transcript then uses `kubectl get pods` to confirm that `client-pod` is running and to read readiness, restart count, and age.
- The service status example emphasizes that the printed `PORT(S)` view exposes the service `port` and the `nodePort`, reinforcing the earlier port-field explanation from [[7 - Service Config Files in Depth]].
- The screenshots make the access path concrete: browser request to node IP plus `nodePort`, not directly to the pod.
- The preface records the practical runtime caveat:

```bash
minikube ip
```

This is necessary for `minikube`, while Docker Desktop may expose the same service through `localhost`.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript presents a strong "never use localhost" rule for `minikube`, but the source's own study-note layer records Docker Desktop as an exception; the wiki should keep treating local-access behavior as runtime-specific rather than universal.
- This lesson shows how to reach a single development workload through `NodePort`, but it does not yet cover the more typical production-facing networking path beyond that pattern.

