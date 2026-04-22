---
title: Why use Services?
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/18 - Why use Services?.md
source_date: 2026-04-19
tags:
  - kubernetes
  - service
  - nodeport
  - pod-ip
  - selector
  - networking
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/7 - Service Config Files in Depth.md
  - wiki/sources/14 - Running Containers with Deployments.md
  - wiki/sources/17 - Applying a  Deployment.md
---

# Why use Services?

## Metadata

- Source type: transcript with embedded screenshots and terminal command/output blocks
- Transcript quality: strong enough to preserve the service motivation, the unstable pod-IP problem, and the selector-based routing explanation
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.58.25 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.58.34 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.59.00 PM.png]]
- Derived notes:
  - `derived/notes/18 - Why use Services? - Cheat Sheet.md`
  - `derived/notes/18 - Why use Services? - Explainer.md`

## Summary

This source explains why Kubernetes needs `Service` objects even after a deployment is already creating the application pods. The main reason is stability: pods can be deleted, recreated, or replaced, and each pod gets its own internal IP address. A service hides that churn behind one stable access point. The lesson revisits the `NodePort` service on port `31515`, shows `kubectl get pods -o wide` to reveal the pod IP, and explains that the service keeps watching pods that match its selector so traffic keeps reaching the right workload even when the underlying pod changes.

## Key Claims

- Every pod gets its own IP address.
- Pod IPs are internal to the node or local cluster environment and are not the right stable address for browser access.
- Pod IPs are not durable because pods can be recreated or replaced at any time.
- A service solves that instability by watching pods that match its selector and routing traffic to them automatically.
- The existing `NodePort` service remains the stable user-facing entry point even when the deployment changes the actual pod instance behind it.
- The course uses the selector `component: web` as the example rule the service relies on.

## Important Evidence Or Examples

- The transcript checks the current pod in wide format:

```bash
kubectl get pods -o wide
```

and the recorded output is:

```text
NAME                                 READY   STATUS    RESTARTS   AGE     IP          NODE             NOMINATED NODE   READINESS GATES
client-deployment-576bc76988-mp7wn   1/1     Running   0          6m50s   10.1.0.15   docker-desktop   <none>           <none>
```

- The transcript then says to use the local cluster node IP plus the hard-coded service port:

```bash
minikube ip
```

and then visit:

```text
<node-ip>:31515
```

- One diagram shows the stable routing path:
  browser -> `kube-proxy` -> `Service` on port `31515` -> pod on port `3000`
- Another diagram shows the instability problem:
  one pod at `172.0.0.1` can disappear and be replaced by another pod at `172.0.0.2`
- The lesson explicitly ties the service back to its selector and says it looks for every pod matching `component: web`.

## Important Commands

```bash
kubectl get pods -o wide
minikube ip
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The spoken explanation assumes a `minikube` VM workflow, but the captured `kubectl get pods -o wide` output shows the node as `docker-desktop`, so the conceptual point is stable while the exact local access path remains runtime-specific.
- The lesson explains why a service is needed, but it still does not show the lower-level mechanism by which Kubernetes keeps the service's target pod list current as pods churn.
