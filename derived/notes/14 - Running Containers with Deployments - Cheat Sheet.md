# Running Containers with Deployments Cheat Sheet

## Source

- Transcript: `raw/transcripts/14 - Running Containers with Deployments.md`
- Wiki source page: [[14 - Running Containers with Deployments]]
- Key visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 3.21.21 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.23.11 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.23.26 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 3.26.27 PM.png]]

## Core Idea

Pods are important to understand, but deployments are the main object used to run containers seriously.

## What A Deployment Is

A deployment is a Kubernetes object that:

- manages one or more identical pods
- keeps them in the correct configuration
- watches for crashes
- recreates or restarts pods when needed

## Pod Versus Deployment

- `Pod`
  one tightly related set of containers
- `Deployment`
  one manager for a set of identical pods

## Why The Course Switches

Direct pod updates are too limited.

That is why the lesson moves from:

- editing pods directly

to:

- letting deployments manage pods

## Pod Template

A deployment contains a pod template.

That template says what every managed pod should look like, including:

```text
container name
image
container port
```

## Main Takeaway

Instead of thinking:

- "I run pods directly"

start thinking:

- "I create a deployment, and the deployment manages pods for me"

## Retrieval Cues

- "deployment manages identical pods"
- "pod template defines each pod"
- "deployments are for serious dev and production use"
- "pods are still important, but not the long-term main interface"

