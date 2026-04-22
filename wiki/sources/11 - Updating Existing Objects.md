---
title: Updating Existing Objects
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/11 - Updating Existing Objects.md
source_date: 2026-04-19
tags:
  - kubernetes
  - declarative
  - object-identity
  - pod
  - kubectl
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/10 - Imperative vs Declarative Deployements.md
  - wiki/analysis/kubernetes-object-identity.md
---

# Updating Existing Objects

## Metadata

- Source type: transcript with embedded screenshots and a practical declarative-update example
- Transcript quality: strong enough to preserve the step-by-step update logic, the image-swap example, and the transcript's simplified rule for how Kubernetes decides between updating an existing object and creating a new one
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 2.46.24 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 2.46.52 PM.png]]
- Referenced visual currently outside canonical `raw/` storage:
  - ![[Screenshot 2026-04-19 at 2.49.24 PM.png]]
- Derived notes:
  - `derived/notes/11 - Updating Existing Objects - Cheat Sheet.md`
  - `derived/notes/11 - Updating Existing Objects - Explainer.md`

## Summary

This source puts the declarative-versus-imperative discussion into practice. The example goal is to update an already running pod so it uses the `multi-worker` image instead of `multi-client`. The transcript argues that the declarative workflow stays the same as always: find the original config file, keep the object name and kind the same, change the desired field in that file, and re-apply it through `kubectl`. It then explains the beginner-level matching rule the course is using: Kubernetes looks at the object's `name` and `kind` to decide whether the new config should update an existing object or create a new one.

## Key Claims

- Updating an existing object declaratively uses the same overall workflow as creation: edit config, then apply it through `kubectl`.
- In the practical example, the desired change is to update the existing pod from `stephengrider/multi-client` to `stephengrider/multi-worker`.
- An imperative approach would require the operator to inspect current pods and then work out the explicit update command.
- A declarative approach reuses the original config file and changes only the desired fields.
- The transcript says every object created from a config file has a `name` and a `kind`.
- The transcript's simplified explanation is that Kubernetes uses matching `name` and `kind` values to decide whether to update an existing object rather than create a new one.
- If `name` changes, the transcript says Kubernetes will treat the config as describing a new object and will create it instead of updating the old one.
- The lesson presents this pattern as the standard way to update existing objects throughout the course.

## Important Evidence Or Examples

- The transcript contrasts two approaches to changing the existing pod image:
  - imperative: list pods, find the current pod, then issue a targeted update command
  - declarative: open the original pod config file, change the image, and re-apply the file
- The source's concrete example is an image swap from `multi-client` to `multi-worker`.
- The transcript repeatedly emphasizes that declarative updates always follow the same process, which is why the course prefers them.
- The matching rule is explained like this in the course's simplified model:
  - Kubernetes reads the config
  - Kubernetes checks the object's `name` and `kind`
  - if both match an existing object, Kubernetes updates that object
  - if not, Kubernetes creates a new object

## Practical Manifest Example

The transcript does not paste the full updated YAML block, but the example change is straightforward based on the earlier pod manifest in [[4 - Adding Config files]]. A normalized illustration of the lesson's update would look like:

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
      image: stephengrider/multi-worker
      ports:
        - containerPort: 3000
```

This block is an explanatory reconstruction of the change the transcript describes, not a verbatim source snippet.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript teaches `name + kind` as the practical update-matching rule for this stage of the course, but it does not discuss namespace, so its identity model is simplified.
- One referenced screenshot is not yet in canonical `raw/assets/` storage, so the source page preserves that evidence with a storage caveat rather than treating it as fully canonical.
- The lesson still works with a direct `Pod` update example, but it does not yet explain the higher-level production object model that usually sits above pods.

