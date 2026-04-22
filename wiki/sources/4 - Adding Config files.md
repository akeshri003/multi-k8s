---
title: Adding Config Files
type: source
status: active
created: 2026-04-17
updated: 2026-04-17
source_path: raw/transcripts/4 - Adding Config files.md
source_date: 2026-04-17
tags:
  - kubernetes
  - yaml
  - pod
  - service
  - nodeport
related:
  - wiki/topics/kubernetes.md
---

# Adding Config Files

## Metadata

- Source type: transcript
- Transcript quality: coherent enough to normalize the first two Kubernetes manifests from the spoken walkthrough
- Derived notes:
  - `derived/notes/4 - Adding Config files - Cheat Sheet.md`
  - `derived/notes/4 - Adding Config files - Explainer.md`

## Summary

This source introduces the first concrete Kubernetes manifest files in the course. It walks through creating one pod manifest for the `multi-client` image and one `NodePort` service manifest to expose that workload outside the cluster.

## Key Claims

- The first practical Kubernetes goal is to get the `multi-client` image running on the local cluster.
- Kubernetes expects the image to already exist on Docker Hub before the manifest is used.
- The first manifest creates the workload object for the client container.
- The second manifest creates a `Service` of type `NodePort` to expose that workload.
- YAML spacing and indentation are operationally important, not cosmetic.

## Important Evidence Or Examples

- The transcript normalizes to a pod manifest like:

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

- The transcript also normalizes to a service manifest like:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: client-node-port
spec:
  type: NodePort
  ports:
    - port: 3050
      targetPort: 3000
      nodePort: 31515
  selector:
    component: web
```

- The two manifests share the `component: web` label/selector pairing, which indicates that the service is intended to target the pod created by the first file.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript creates a `Pod` directly, but it does not yet explain whether this is the long-term production pattern or just the first teaching example.
- The lesson still postpones the deeper explanation of why Kubernetes uses object kinds instead of a single Compose-like service declaration.
