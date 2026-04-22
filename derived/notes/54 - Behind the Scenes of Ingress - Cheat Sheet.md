# Behind the Scenes of Ingress Cheat Sheet

## Source

- Transcript: `raw/transcripts/54 - Behind the Scenes of Ingress.md`
- Wiki source page: [[54 - Behind the Scenes of Ingress]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 12.39.03 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.40.08 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.41.13 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.44.15 PM.png]]

## Core Idea

Ingress works like other Kubernetes controller patterns:

- config describes desired routing
- controller makes that routing real

## The Ingress Pattern

- ingress config = routing rules
- ingress controller = watches and enforces those rules
- routing implementation = actually accepts traffic and forwards it

## Ingress-Nginx Specific Note

- in ingress-nginx, controller and router are effectively packaged together

## Retrieval Cues

- "ingress is a controller pattern"
- "config first, controller second"
- "ingress-nginx merges controller + routing"
