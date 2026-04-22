---
title: Imperatively Updating a Deployment's Image
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/23 - Imperatively Updating a Deployment's Image.md
source_date: 2026-04-19
tags:
  - kubernetes
  - deployment
  - kubectl
  - set-image
  - imperative
  - image-tags
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/21 - Rebuilding the Client Image.md
  - wiki/sources/22 - Triggering Deployment Updates.md
---

# Imperatively Updating a Deployment's Image

## Metadata

- Source type: transcript with embedded diagrams and command workflow
- Transcript quality: strong enough to preserve the version-tag rebuild flow, the full `kubectl set image` command structure, and the browser verification steps
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 5.42.59 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 5.47.49 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 5.49.33 PM.png]]
- Derived notes:
  - `derived/notes/23 - Imperatively Updating a Deployment's Image - Cheat Sheet.md`
  - `derived/notes/23 - Imperatively Updating a Deployment's Image - Explainer.md`

## Summary

This source turns the previous lesson's preferred workaround into a concrete workflow. It rebuilds the `multi-client` image with an explicit version tag such as `v5`, pushes that tagged image to Docker Hub, and then uses `kubectl set image` to imperatively update the `client-deployment` object so its `client` container points at the new image tag. The lesson verifies the update by checking that the pod age resets and by confirming in the browser that `Fib calculator version two` eventually appears.

## Key Claims

- Tagging the rebuilt image with a distinct version number is the first step in making image updates explicit.
- After the tagged image is pushed, `kubectl set image` can directly update the deployment's image property without editing the manifest file.
- The command targets a specific object type, object name, and container name before assigning the new full image reference.
- A new low pod age after the command is practical evidence that the deployment recreated the pod.
- Browser content is used as the final proof that the new image version actually reached the running workload.
- React/browser caching can briefly hide the change even when the deployment update already succeeded.

## Important Evidence Or Examples

- The transcript rebuilds the image with an explicit version tag:

```bash
docker build -t <docker-id>/multi-client:v5 .
```

- It then pushes that tagged image:

```bash
docker push <docker-id>/multi-client:v5
```

- The central Kubernetes command is:

```bash
kubectl set image deployment/client-deployment client=<docker-id>/multi-client:v5
```

- The lesson explains each part of that command:
  - `set` means update a property on an existing cluster object
  - `image` is the property being updated
  - `deployment/client-deployment` identifies the object to change
  - `client` identifies the container inside the pod template
  - `<docker-id>/multi-client:v5` is the full new image reference
- After the command, `kubectl get pods` is used to check that the single pod has a very small age, which the lesson reads as evidence that it was recreated.
- The browser is then used to confirm that the updated app text is:

```text
Fib calculator version two
```

- If the browser does not show the new text immediately, the transcript suggests:
  - refresh after a short wait
  - disable cache in Chrome DevTools Network tab
  - rerun the `kubectl set image` command if needed

## Important Commands

```bash
docker build -t <docker-id>/multi-client:v5 .
docker push <docker-id>/multi-client:v5
kubectl set image deployment/client-deployment client=<docker-id>/multi-client:v5
kubectl get pods
minikube ip
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The course now has a concrete image-update recipe, but it depends on an imperative command rather than a purely declarative manifest update.
- The lesson treats the manual command sequence as acceptable because later deployment scripts will automate it, but that means the clean source-of-truth story now depends partly on external scripting.
- The runtime-specific browser-access caveat still applies: the transcript uses `minikube ip`, while nearby material has also shown `docker-desktop`.
