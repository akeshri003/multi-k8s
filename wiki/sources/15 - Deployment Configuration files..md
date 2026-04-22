---
title: Deployment Configuration files.
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/15 - Deployment Configuration files..md
source_date: 2026-04-19
tags:
  - kubernetes
  - deployment
  - yaml
  - pod-template
  - apps-v1
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/14 - Running Containers with Deployments.md
---

# Deployment Configuration files.

## Metadata

- Source type: transcript with an embedded YAML block and supporting screenshot
- Transcript quality: strong enough to preserve the first real deployment manifest and the role of its main sections
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.35.07 PM.png]]
- Derived notes:
  - `derived/notes/15 - Deployment Configuration files.. - Cheat Sheet.md`
  - `derived/notes/15 - Deployment Configuration files.. - Explainer.md`

## Summary

This source turns the previous conceptual deployment discussion into a real Kubernetes manifest. It creates a `Deployment` named `client-deployment` that manages one replica, matches pods labeled `component: web`, and uses a pod template that runs the `stephengrider/multi-client` image with container port `3000`. The lesson says that from this point forward the course will stop creating pods manually and will instead create deployments that create pods.

## Key Claims

- Deployments are now the standard object the course will use instead of creating pods manually.
- A deployment is created the same general way as other Kubernetes objects: write a configuration file and apply it with `kubectl`.
- This deployment uses `apiVersion: apps/v1` and `kind: Deployment`.
- The deployment is named `client-deployment`.
- `replicas: 1` means the deployment should maintain one pod.
- `selector.matchLabels` identifies which pods belong to the deployment.
- `template` contains the pod template that defines what each managed pod should look like.
- The pod template in this lesson runs one container named `client`, using the `stephengrider/multi-client` image, and exposes container port `3000`.

## Important Evidence Or Examples

- The transcript gives the full deployment manifest:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: client-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: web
  template:
    metadata:
      labels:
        component: web
    spec:
      containers:
        - name: client
          image: stephengrider/multi-client
          ports:
            - containerPort: 3000
```

- The transcript explains that some parts should already feel familiar:
  - `apiVersion` and `kind` choose the object type
  - `metadata.name` names the deployment
  - the inner container block resembles the earlier pod manifest
- The new parts that matter most are:
  - `replicas`
  - `selector.matchLabels`
  - `template`
- The reason for using `multi-client` and port `3000` here is practical: the course wants a browser-testable workload while switching from direct pods to deployments.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The lesson provides the first deployment YAML, but it postpones the detailed line-by-line explanation of how `selector` and `template` interact.
- The source makes the transition away from direct pod creation explicit, but it has not yet shown how the existing `Service` should connect to the deployment-managed pods.
- The filename itself uses a double period, which is preserved here to stay aligned with the raw source naming convention.

