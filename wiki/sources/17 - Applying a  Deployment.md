---
title: Applying a Deployment
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/17 - Applying a  Deployment.md
source_date: 2026-04-19
tags:
  - kubernetes
  - deployment
  - kubectl
  - delete
  - get-deployments
  - status
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/15 - Deployment Configuration files..md
  - wiki/sources/16 - Walkthrough Deployment Config.md
---

# Applying a Deployment

## Metadata

- Source type: transcript with embedded screenshot and terminal command/output blocks
- Transcript quality: strong enough to preserve the deployment-apply workflow, the cleanup step for the old pod, and the meaning of the deployment status columns
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.44.56 PM.png]]
- Derived notes:
  - `derived/notes/17 - Applying a  Deployment - Cheat Sheet.md`
  - `derived/notes/17 - Applying a  Deployment - Explainer.md`

## Summary

This source applies the first deployment to the cluster. Before doing that, it deletes the older directly created pod with `kubectl delete -f client-pod.yaml` so the cluster state is easier to read. It then applies `client-deployment.yaml`, confirms that a new pod appears with a deployment-generated name, and introduces `kubectl get deployments` as the status view for deployment objects. The lesson also explains the meaning of the `desired`, `current`, `up-to-date`, and `available` deployment columns.

## Key Claims

- Before applying the deployment, it is cleaner to remove the older manually created pod.
- `kubectl delete -f <file>` uses the object's `kind` and `name` from the manifest to find and delete the matching object.
- The transcript explicitly calls `kubectl delete` an imperative operation, in contrast with the course's usual declarative preference.
- The course treats deletion as one of the places where an imperative command is still necessary or at least practical.
- Applying `client-deployment.yaml` creates a deployment, which in turn creates a pod.
- The created pod now has a generated name that is clearly tied to the deployment rather than the older fixed `client-pod` name.
- `kubectl get deployments` is the new status view for deployment objects.
- In the deployment status table:
  - `desired` is the requested pod count from `replicas`
  - `current` is the number of pods currently running
  - `up-to-date` tracks whether pods reflect the latest deployment template
  - `available` counts pods that are ready to serve traffic

## Important Evidence Or Examples

- The cleanup step is:

```bash
kubectl get pods
kubectl delete -f client-pod.yaml
```

and the recorded result is:

```text
pod "client-pod" deleted from default namespace
```

- The transcript explains that deletion takes time because the running pod is given time to shut down before being forcibly removed.
- The deployment-apply step is:

```bash
kubectl apply -f client-deployment.yaml
```

- The transcript then checks:

```bash
kubectl get pods
kubectl get deployments
```

- The lesson says the new pod's name is now randomly generated in a way that is clearly tied to the deployment.
- The deployment status columns are interpreted like this:
  - `desired`: target number of replicas
  - `current`: pods currently existing
  - `up-to-date`: pods using the latest template
  - `available`: pods successfully running and able to accept traffic

## Important Commands

The transcript's practical command sequence is:

```bash
kubectl get pods
kubectl delete -f client-pod.yaml
kubectl get pods
kubectl apply -f client-deployment.yaml
kubectl get pods
kubectl get deployments
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The lesson introduces deletion as an imperative escape hatch, which slightly complicates the course's strong declarative framing.
- The source explains deployment status columns, but it still postpones the next step of making sure the browser can reach the workload through the existing service arrangement.
- The transcript suggests that `up-to-date` will become important once the deployment template changes, but it has not yet shown that flow in practice.

