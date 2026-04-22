---
title: Updating Deployment Images
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/20 - Updating Deployment Images.md
source_date: 2026-04-19
tags:
  - kubernetes
  - deployment
  - image-updates
  - docker-hub
  - multi-client
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/19 - Scaling and Chaning Deployments.md
  - wiki/sources/21 - Rebuilding the Client Image.md
  - wiki/sources/22 - Triggering Deployment Updates.md
---

# Updating Deployment Images

## Metadata

- Source type: transcript with embedded planning diagram and command workflow
- Transcript quality: strong enough to preserve the three-step image-update plan and the deployment reset back to a browser-testable state
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 5.23.36 PM.png]]
- Derived notes:
  - `derived/notes/20 - Updating Deployment Images - Cheat Sheet.md`
  - `derived/notes/20 - Updating Deployment Images - Explainer.md`

## Summary

This source sets up the next image-update sequence. It switches the deployment back from `multi-worker` to `multi-client`, reduces replicas to `1`, and restores the container port to `3000` so the browser test path is easy to verify again. The lesson then outlines the three-step plan: point the deployment back at `multi-client`, change and rebuild the `multi-client` image, push it to Docker Hub, and then somehow force the deployment to recreate its pods using the latest image version.

## Key Claims

- Updating a deployment because a new image version exists is a separate problem from simply editing fields like `replicas` or `image` in the manifest.
- The course temporarily switches back to `multi-client` because it gives a visible browser result that makes version verification easier.
- Reducing replicas to `1` makes it easier to observe whether the single pod gets recreated or updated.
- Restoring `containerPort` to `3000` is part of returning the workload to a browser-testable state.
- The real workflow the course wants to simulate is:
  - update deployment to use `multi-client`
  - rebuild and push a changed `multi-client` image
  - force the deployment to recreate pods from the latest image

## Important Evidence Or Examples

- The transcript says the deployment file is changed in three concrete ways:
  - `replicas` goes back to `1`
  - the image changes from `multi-worker` to `multi-client`
  - `containerPort` changes back to `3000`
- The deployment is then re-applied with:

```bash
kubectl apply -f client-deployment.yaml
```

- The lesson verifies browser access again by getting the local cluster IP and visiting the NodePort-backed route:

```bash
minikube ip
```

and then:

```text
<node-ip>:31515
```

- The embedded diagram summarizes the whole sequence as:
  - change deployment to use `multi-client` again
  - update the `multi-client` image and push to Docker Hub
  - get the deployment to recreate pods with the latest version of `multi-client`

## Important Commands

```bash
kubectl apply -f client-deployment.yaml
minikube ip
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This source sets up the image-update problem but does not yet solve how Kubernetes should notice a newer Docker Hub image when the deployment file itself has not changed.
- The lesson still uses the older `minikube` browser-access framing even though nearby source material has also shown `docker-desktop`, so the access path remains runtime-specific.
