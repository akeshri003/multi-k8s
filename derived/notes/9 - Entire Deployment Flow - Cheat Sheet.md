# Entire Deployment Flow Cheat Sheet

## Source

- Transcript: `raw/transcripts/9 - Entire Deployment Flow.md`
- Wiki source page: [[9 - Entire Deployment Flow]]
- Key visuals:
  - ![[raw/assets/Screenshot 2026-04-17 at 11.07.03 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.10.28 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.20.18 PM.png]]

## Core Idea

Kubernetes is not just "start a container once."

It keeps checking:

- what the cluster should be running
- what the cluster is actually running
- what needs to be created again if something disappears

## Main Flow

1. You write a config or deployment file.
2. You send it with `kubectl`.
3. The file goes to the master, not directly to a node.
4. The master reads the desired state.
5. The master tells nodes how many copies to run.
6. Nodes pull the image and start containers.
7. The master keeps checking status.
8. If one copy dies, Kubernetes recreates it.

## Demo Commands

```bash
kubectl get pods
docker ps
docker kill <container-id>
```

## What The Demo Proves

- killing the container does not end the workload permanently
- the container comes back
- the pod restart count increases
- Kubernetes is trying to maintain the requested state

## Example In The Lesson

The source imagines a deployment that wants:

```text
4 copies of multi-worker
```

The master can spread those copies across several nodes, such as:

```text
node A -> 1
node B -> 1
node C -> 2
```

## Important Terms

- `kubectl`
  The tool the developer uses to talk to Kubernetes.
- `master`
  The lesson's older term for the control machine.
- `Kube API Server`
  The simplified control component that reads config and tracks desired state.
- `node`
  A machine or VM that runs workloads.
- `desired state`
  What Kubernetes has been told should exist.
- `actual state`
  What is really running right now.

## Retrieval Cues

- "send config to the master"
- "master compares desired vs actual state"
- "nodes pull images and run containers"
- "if a container dies, Kubernetes recreates it"

