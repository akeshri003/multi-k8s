# Load Balancer Services Cheat Sheet

## Source

- Transcript: `raw/transcripts/51 - Load Balancer Services.md`
- Wiki source page: [[51 - Load Balancer Services]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 12.29.09 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.30.29 PM.png]]

## Core Idea

`LoadBalancer` can expose one set of pods, but this app needs more flexible outside routing.

That is why the course moves to `Ingress`.

## Service Type Refresher

- `ClusterIP`
  - inside-cluster access
- `NodePort`
  - outside access for development
- `LoadBalancer`
  - older cloud-facing outside access
- `Ingress`
  - outside routing across services

## Why Not Use `LoadBalancer` Here

- it exposes one set of pods
- this app needs to route outside traffic to multiple destinations
- client and server both matter

## Retrieval Cues

- "`LoadBalancer` = one exposed target set"
- "`Ingress` = better routing fit"
- "cloud provider creates the external load balancer"
