---
title: Object Types and API Versions
type: source
status: active
created: 2026-04-17
updated: 2026-04-17
source_path: raw/transcripts/5 - Object Types and API Versions.md
source_date: 2026-04-17
tags:
  - kubernetes
  - yaml
  - api-version
  - kind
  - pod
  - service
related:
  - wiki/topics/kubernetes.md
---

# Object Types and API Versions

## Metadata

- Source type: transcript with embedded code blocks and screenshots
- Transcript quality: strong enough to preserve both the conceptual explanation and the pasted manifest examples
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-17 at 7.31.05 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 7.32.11 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 7.37.29 PM.png]]
- Derived notes:
  - `derived/notes/5 - Object Types and API Versions - Cheat Sheet.md`
  - `derived/notes/5 - Object Types and API Versions - Explainer.md`

## Summary

This source explains the first two conceptual keys in Kubernetes manifests: `kind` and `apiVersion`. It frames Kubernetes config files as declarations for creating cluster objects, then explains that `kind` selects the object type while `apiVersion` determines which set of object types the manifest can use.

## Key Claims

- Kubernetes config files create objects in the cluster rather than directly functioning like a Compose service list.
- `kind` identifies the object type being created, such as `Pod` or `Service`.
- Different object types serve different roles, such as running containers or setting up networking.
- `apiVersion` scopes which object kinds are available in a manifest.
- In practice, the usual workflow is to choose the object kind first, then look up the matching API version.

## Important Evidence Or Examples

- The source uses the earlier manifests as examples:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: client-pod
  labels:
    component: web
spec:
  containers:
    - name: client
      image: stephengrider/multi-client
      ports:
        - containerPort: 3000
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: client-node-port
spec:
  type: NodePort
  ports:
    - ports: 3050
      targetPort: 3000
      nodePort: 31515
  selector:
    component: web
```

- The service example above appears to contain a likely typo in the source block: `ports: 3050`. Based on the earlier walkthrough in [[4 - Adding Config files]], the intended field is likely `port: 3050`. This is an inference, not a direct source claim.
- One screenshot shows config files as inputs to Kubernetes-style objects such as `Pod` and `Service`.
- Another screenshot explains that `apiVersion: v1` and `apiVersion: apps/v1` expose different sets of object types.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source clarifies that `Pod` and `Service` are object kinds, but it still does not resolve which object kind is the best long-term analogue to a typical Docker Compose service in production.
- The `apiVersion` workflow is explained as lookup-driven and somewhat reactive, which suggests future notes should capture common object-kind to API-version mappings explicitly.
