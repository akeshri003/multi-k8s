# NodePort vs ClusterIP Services Cheat Sheet

## Source

- Transcript: `raw/transcripts/27 - NodePort vs ClusterIP Services.md`
- Wiki source page: [[27 - NodePort vs ClusterIP Services]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 8.21.07 PM.png]]

## Core Idea

This lesson explains the difference between two service types:

- `NodePort`
- `ClusterIP`

## `NodePort`

- exposes pods to the outside world
- that is why earlier demos worked through a node IP plus a port in the `30000` range
- the course treats it as a development-oriented access pattern

## `ClusterIP`

- exposes pods only to other objects inside the cluster
- outside users cannot directly use it
- good for internal app-to-app communication

## Practical Example

- worker should be able to reach Redis
- Redis should not be directly exposed to outside users
- so Redis is a good fit for `ClusterIP`

## Where `Ingress` Fits

- outside traffic enters through `Ingress`
- once traffic is inside the cluster, `ClusterIP` services can route it internally

## Simple Comparison

- `NodePort`
  outside world -> service -> pods
- `ClusterIP`
  inside cluster -> service -> pods

## Retrieval Cues

- "NodePort is outside-facing"
- "ClusterIP is internal-only"
- "worker to Redis is a ClusterIP example"
- "Ingress is the outer entry point in the new architecture"
