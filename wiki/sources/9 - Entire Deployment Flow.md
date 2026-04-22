---
title: Entire Deployment Flow
type: source
status: active
created: 2026-04-17
updated: 2026-04-17
source_path: raw/transcripts/9 - Entire Deployment Flow.md
source_date: 2026-04-17
tags:
  - kubernetes
  - deployment
  - kubectl
  - kube-api-server
  - reconciliation
  - self-healing
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/8 - Connecting to Running Containers.md
---

# Entire Deployment Flow

## Metadata

- Source type: transcript with embedded screenshots and command-line demonstration
- Transcript quality: strong enough to preserve both the visual architecture explanation and the operational claims about scheduling, image pulling, and restart behavior
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-17 at 11.07.03 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.10.28 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.20.18 PM.png]]
- Derived notes:
  - `derived/notes/9 - Entire Deployment Flow - Cheat Sheet.md`
  - `derived/notes/9 - Entire Deployment Flow - Explainer.md`

## Summary

This source explains what happens behind the scenes after a Kubernetes configuration file is applied. It uses an imagined deployment that requests four copies of a `multi-worker` workload and walks through the control flow: the file goes to the master, the simplified `Kube API Server` interprets the desired state, the master assigns work across nodes, each node pulls the needed image and creates containers, and the master keeps checking until the actual state matches the requested state. The lesson also uses a live `docker kill` demo to show that Kubernetes notices a crashed container and recreates it automatically.

## Key Claims

- Developers do not send work directly to worker nodes; they interact with the master through `kubectl`.
- The source simplifies the master into one main control program, the `Kube API Server`, even though it says multiple programs exist in reality.
- The master keeps a running record of what should exist in the cluster and compares that against what is actually running.
- When a deployment asks for multiple copies of a workload, the master can distribute those copies across several nodes.
- Each node runs Docker locally and can pull container images from Docker Hub when instructed.
- The containers are conceptually running inside pods, even if the diagram omits pod detail for simplicity.
- If a container dies, the master notices the mismatch and arranges for a replacement so the desired number of copies is restored.
- This ongoing comparison between desired state and actual state is one of the central ideas in Kubernetes.

## Important Evidence Or Examples

- The transcript begins with a live demo that uses `kubectl get pods`, `docker ps`, and `docker kill` to show that a killed container reappears and that pod restart count increases.
- The first screenshot frames the lesson as a move from a simple architecture sketch to a more realistic deployment-flow view.
- The larger deployment-flow diagrams show this sequence:
  1. a deployment file is applied with `kubectl`
  2. the file reaches the master
  3. the simplified `Kube API Server` reads the requested state
  4. the master updates its internal responsibility list
  5. the master tells nodes to start the required number of workload copies
  6. nodes pull the image from Docker Hub
  7. nodes create containers from that image
  8. nodes report status back
  9. the master keeps checking whether actual state matches desired state
- The example workload is an imagined deployment that asks for four copies of `multi-worker`, split across three nodes.
- The transcript explicitly notes that the master is always watching nodes, not just acting once and stopping.
- The source uses older terminology such as `master`, and its architecture explanation is intentionally simplified for teaching purposes.

## Important Commands

The transcript references these commands as part of the demonstration:

```bash
kubectl get pods
docker ps
docker kill <container-id>
```

These are presented mainly as a visual demo of restart behavior, not as the normal developer workflow for managing Kubernetes workloads.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source uses `master` terminology and a simplified `Kube API Server` story, so future sources may need to map this explanation onto modern control-plane terminology and the fuller component model.
- The lesson explains desired-state reconciliation conceptually, but it does not yet name the higher-level Kubernetes objects that usually manage replica counts in modern practice.

