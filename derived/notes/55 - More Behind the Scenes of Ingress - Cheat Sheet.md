# More Behind the Scenes of Ingress Cheat Sheet

## Source

- Transcript: `raw/transcripts/55 - More Behind the Scenes of Ingress.md`
- Wiki source page: [[55 - More Behind the Scenes of Ingress]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 12.46.11 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.54.06 PM.png]]

## Core Idea

On Google Cloud, ingress-nginx ends up creating more infrastructure than just one ingress object.

## Google Cloud Flow

- ingress config
- ingress-nginx deployment/pod
- Google Cloud load balancer
- in-cluster `LoadBalancer` service
- routing to client/server services
- default-backend for health checks

## Important Twist

- even though ingress is preferred, a `LoadBalancer` service still exists behind the scenes in this setup

## Retrieval Cues

- "Google Cloud load balancer outside"
- "`LoadBalancer` service inside"
- "default-backend does health checks"
