---
title: Mapping Existing Knowledge
type: source
status: active
created: 2026-04-17
updated: 2026-04-17
source_path: raw/transcripts/3 - Mapping Existing Knowledge.md
source_date: 2026-04-17
tags:
  - kubernetes
  - docker-compose
  - networking
  - images
related:
  - wiki/topics/kubernetes.md
---

# Mapping Existing Knowledge

## Metadata

- Source type: transcript with referenced screenshots
- Transcript quality: coherent enough to preserve the lesson's main conceptual mapping
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-17 at 6.38.24 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 6.40.58 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 6.44.26 PM.png]]

## Summary

This source translates familiar Docker Compose ideas into Kubernetes terms. Its central point is that Kubernetes does not provide a build pipeline, does not revolve around a single config file that maps directly to containers, and requires much more explicit networking setup than Docker Compose.

## Key Claims

- Docker Compose entries can optionally build images, but Kubernetes expects images to already be built before deployment.
- Docker Compose entries represent containers, whereas Kubernetes works through multiple configuration files that create different objects.
- Docker Compose can express much of the needed networking in one file, but Kubernetes requires more explicit manual networking setup.
- A simple first goal on a local cluster is to get the `multi-client` image running.
- That goal breaks into three steps: confirm the image is on Docker Hub, create one config file for the workload object, and create a second config file for networking.

## Important Evidence Or Examples

- One screenshot summarizes the Docker Compose side as: optional image builds, each entry representing a container, and each entry defining ports.
- A second screenshot maps those same ideas into Kubernetes: prebuilt images, one config file per object, and manual networking.
- A third screenshot turns that mapping into an execution plan for the `multi-client` image: image availability, one config for the container-like workload, and one config for networking.
- The transcript explicitly says Kubernetes has "no build pipeline" and that a "vast majority" of later work will focus on networking.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The transcript says Kubernetes creates "objects" rather than directly creating containers, but it postpones the explanation of what specific object will actually be used.
- The networking explanation is motivational rather than precise; the source does not yet say which Kubernetes networking primitives will be involved.
