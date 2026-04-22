---
title: Walkthrough Deployment Config
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/16 - Walkthrough Deployment Config.md
source_date: 2026-04-19
tags:
  - kubernetes
  - deployment
  - yaml
  - selector
  - template
  - replicas
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/15 - Deployment Configuration files..md
---

# Walkthrough Deployment Config

## Metadata

- Source type: transcript with embedded screenshots and a conceptual walkthrough of deployment YAML
- Transcript quality: strong enough to preserve the meaning of the deployment file sections and the relation between `selector`, `template`, and the created pods
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.38.36 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.41.19 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.41.34 PM.png]]
- Derived notes:
  - `derived/notes/16 - Walkthrough Deployment Config - Cheat Sheet.md`
  - `derived/notes/16 - Walkthrough Deployment Config - Explainer.md`

## Summary

This source walks line by line through the first deployment manifest. It explains that `template` contains the pod configuration used for every pod created by the deployment, that `replicas` controls how many identical pods should exist, and that `selector.matchLabels` gives the deployment a way to find and keep track of the pods it asked Kubernetes to create. The lesson frames the template as a familiar pod-like block nested inside a higher-level deployment object.

## Key Claims

- `apiVersion` and `kind` still do the same basic job as before: they choose the object type and API group/version.
- `metadata.name` names the deployment object itself.
- `template` defines the exact configuration for every pod created by the deployment.
- The pod template should look familiar because it contains the same kinds of pod-level fields used in the earlier bare pod manifest.
- `replicas` specifies how many identical pods the deployment should maintain.
- `selector.matchLabels` is how the deployment finds the pods that belong to it after they are created.
- The selector is similar in spirit to the selector used earlier in the `Service` config, even though it is serving a different controller purpose here.
- The lesson says the repeated label fields are not accidental duplication; the deployment needs a matching mechanism to find the pods it manages.

## Important Evidence Or Examples

- The transcript reuses the deployment manifest from the previous lesson and explains the sections rather than introducing a brand-new file.
- The core explanation of `template` is:

```text
every pod created by this deployment should use this pod configuration
```

- The core explanation of `replicas` is:

```text
create and maintain this many identical pods
```

- The core explanation of `selector.matchLabels` is:

```text
find the pods with these labels so the deployment can keep track of the pods it asked Kubernetes to create
```

- The lesson explicitly compares the deployment selector to the earlier service selector to make the idea easier to recognize.
- It also notes that the repeated `component: web` label is necessary so the deployment's selector has something stable to match against.

## Reference Manifest

The transcript is explaining this deployment file:

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

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source explains why `selector` and template labels must line up, but it still stays at a simplified conceptual level and does not yet show what happens if they do not match.
- The lesson focuses on how the deployment finds the pods it manages, but it still does not explain how the existing `Service` should connect to deployment-created pods.
- The controller logic is still described in simplified “deployment asks master to create a pod and then gets a handle on it” terms rather than the fuller Kubernetes controller model.

