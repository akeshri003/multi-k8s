---
title: Triggering Deployment Updates
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/22 - Triggering Deployment Updates.md
source_date: 2026-04-19
tags:
  - kubernetes
  - deployment
  - image-updates
  - kubectl
  - version-tags
  - imperative
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/20 - Updating Deployment Images.md
  - wiki/sources/21 - Rebuilding the Client Image.md
---

# Triggering Deployment Updates

## Metadata

- Source type: transcript with embedded diagrams and conceptual command workflow
- Transcript quality: strong enough to preserve why unchanged manifests do not trigger image refreshes and the three workaround families the course compares
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 5.30.46 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 5.34.45 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 5.37.28 PM.png]]
- Derived notes:
  - `derived/notes/22 - Triggering Deployment Updates - Cheat Sheet.md`
  - `derived/notes/22 - Triggering Deployment Updates - Explainer.md`

## Summary

This source explains why image updates are awkward in Kubernetes when the deployment manifest still points at the same image tag. If the file has not changed, `kubectl apply -f` treats it as unchanged and does not go look for a newer image in Docker Hub. The lesson references Kubernetes GitHub issue `#33664` and then compares three workaround families: manually delete pods so the deployment recreates them, use real version tags and update the config file each time, or use a versioned image plus an imperative command that tells Kubernetes to update the deployment image directly.

## Key Claims

- `kubectl apply -f` only reacts to config changes; it does not treat "a newer image now exists in Docker Hub" as a meaningful change by itself.
- Re-applying an unchanged deployment file is rejected as unchanged rather than causing pods to refresh.
- Kubernetes does not automatically infer "use the latest image contents" when the manifest still points at the same image reference.
- The course presents three workaround families:
  - manually delete managed pods and let the deployment recreate them
  - tag images with real version numbers and update the deployment config to point at the new version
  - tag images with real version numbers and then use an imperative command to update the deployment image directly
- The course considers all three options imperfect, but treats the third as the most practical of the bad options.

## Important Evidence Or Examples

- The transcript uses the unchanged-manifest problem as the core demonstration:

```bash
kubectl apply -f client-deployment.yaml
```

- The lesson says this is rejected as unchanged when the file contents are identical to the last applied version.
- The transcript points to Kubernetes GitHub issue:

```text
Issue #33664
```

- The first workaround is:
  manually delete the pods and let the deployment recreate them
- The second workaround is:
  build and push versioned images such as `v1`, `v2`, `v3`, then edit the config file so the manifest points at the new tag
- The third workaround is:
  still build versioned images, but skip editing the config file directly and instead run an imperative command that updates the deployment image version in the cluster
- The lesson explicitly says environment variables are not allowed inside these config files as an easy workaround for injecting the image version number.
- The lesson also argues that updating the config file on every image build becomes especially painful in CI because the image build and the config-file version bump would need awkward coordination and extra commits.

## Important Commands

Concrete command shown:

```bash
kubectl apply -f client-deployment.yaml
```

Concrete imperative update command is not yet shown in this transcript; it is only described conceptually as the next lesson's direction.

## Related Topics

- [[kubernetes]]

## Related Entities

- Kubernetes issue `#33664`

## Tensions Or Open Questions

- The lesson treats "latest image" updates as a bad fit for purely declarative `kubectl apply` workflows when the manifest text does not change.
- The preferred workaround still uses an imperative command, which cuts against the course's general declarative preference.
- The exact imperative command is deferred to the next lesson, so this source ends at the conceptual comparison stage rather than the final working recipe.
