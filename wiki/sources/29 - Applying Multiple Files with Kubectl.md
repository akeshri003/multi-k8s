---
title: Applying Multiple Files with Kubectl
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/29 - Applying Multiple Files with Kubectl.md
source_date: 2026-04-19
tags:
  - kubernetes
  - kubectl
  - deployment
  - service
  - clusterip
  - workflow
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/26 - Recreating the Deployment.md
  - wiki/sources/28 - The ClusterIP Config.md
---

# Applying Multiple Files with Kubectl

## Metadata

- Source type: transcript with command-line walkthrough and embedded terminal output
- Transcript quality: strong enough to preserve the cleanup workflow, the multi-file `kubectl apply` shortcut, the deployment YAML error, and the successful re-apply
- Embedded visuals:
  - None explicitly provided in the transcript or newly added assets
- Derived notes:
  - `derived/notes/29 - Applying Multiple Files with Kubectl - Cheat Sheet.md`
  - `derived/notes/29 - Applying Multiple Files with Kubectl - Explainer.md`

## Summary

This source shows how to apply more than one Kubernetes config file at once by pointing `kubectl apply -f` at the whole `k8s/` directory instead of naming files one by one. Before doing that, the lesson deletes the older `client-deployment` and `client-node-port` resources so the new frontend deployment plus `ClusterIP` service can be created cleanly. The transcript also captures a very practical debugging moment: the first apply fails because of a typo in the deployment manifest, the typo is fixed, and then the directory-wide apply succeeds.

## Key Claims

- It is often useful to apply an entire directory of config files instead of applying each file separately.
- `kubectl apply -f k8s` causes `kubectl` to find and apply every config file inside the `k8s/` directory.
- Before applying the new frontend deployment and service, the old `client-deployment` and `client-node-port` resources should be removed.
- The new `client-cluster-ip-service` can be created even before browser testing is possible, because `ClusterIP` is internal-only.
- A failed apply can still partially succeed for some resources while failing for others.
- YAML mistakes in one file should be corrected and then the directory can be re-applied.
- After a successful apply, `kubectl get deployments`, `kubectl get pods`, and `kubectl get services` are used to confirm the new resources exist.

## Important Evidence Or Examples

- The cleanup flow is:

```bash
kubectl get deployments
kubectl delete deployment client-deployment
kubectl get deployments
kubectl get services
kubectl delete service client-node-port
kubectl get services
```

- The directory-wide apply shortcut is:

```bash
kubectl apply -f k8s
```

- The first apply partially succeeds:

```text
service/client-cluster-ip-service created
Error from server (BadRequest): error when creating "k8s/client-deployment.yaml": Deployment in version "v1" cannot be handled as a Deployment: json: cannot unmarshal string into Go struct field Container.spec.template.spec.containers.ports of type v1.ContainerPort
```

- The transcript explains the cause as a typo in the deployment file, specifically `metadata label` instead of `metadata labels`.
- After fixing the typo and re-applying, the service stays unchanged and the deployment is created:

```text
service/client-cluster-ip-service unchanged
deployment.apps/client-deployment created
```

- The verification flow after the successful apply is:

```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

- The successful deployment output shown in the transcript is:

```text
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
client-deployment   3/3     3            3           14s
```

- The transcript explicitly says browser testing is not possible yet because the frontend is now behind a `ClusterIP` service rather than an externally reachable service.

## Important Commands

```bash
kubectl get deployments
kubectl delete deployment client-deployment
kubectl get services
kubectl delete service client-node-port
kubectl apply -f k8s
kubectl get pods
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript verbally blames the first failed apply on `metadata label` versus `labels`, but the shown server error mentions the `containers.ports` field, so the exact original deployment typo is not fully trustworthy in the transcript.
- The lesson confirms the internal resources exist, but it still cannot demonstrate end-to-end browser access because `Ingress` has not been added yet.
- The source shows directory-wide apply as a useful shortcut, but it does not yet discuss how teams organize many manifest files as the config set grows larger.
