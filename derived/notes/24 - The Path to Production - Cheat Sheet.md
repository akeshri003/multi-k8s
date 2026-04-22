# The Path to Production Cheat Sheet

## Source

- Transcript: `raw/transcripts/24 - The Path to Production.md`
- Wiki source page: [[24 - The Path to Production]]

## Key Visuals

- ![[raw/assets/Pasted image 20260419201828.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 8.18.14 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 6.24.15 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 6.29.03 PM.png]]

## Core Idea

This lesson starts the move from isolated Kubernetes examples to the full multi-container Fibonacci app.

The new architecture includes:

- `multi-client`
- `multi-server`
- `multi-worker`
- `Redis`
- `Postgres`
- `Ingress Service`
- multiple `ClusterIP` services
- `Postgres PVC`

## Big Shift

Earlier:

- Redis and Postgres were treated as outside managed services in the Elastic Beanstalk setup

Now:

- Redis and Postgres will be run inside Kubernetes

## New Terms Introduced

- `Ingress Service`
  new traffic-entry concept for the app
- `ClusterIP`
  internal service type used inside the cluster
- `PVC`
  `Persistent Volume Claim`, introduced here for Postgres storage

## Path To Production

1. create config files for each service, deployment, and storage object
2. test everything locally on `minikube`
3. create a GitHub/Travis flow to build images and deploy
4. deploy to AWS or Google Cloud

## Why This Lesson Matters

- the course is no longer teaching single isolated Kubernetes objects
- it is now teaching how a real multi-service app fits together
- the next chunk of work will involve many config files rather than one or two simple manifests

## Retrieval Cues

- "full Fibonacci app moves into Kubernetes"
- "Ingress + ClusterIP + PVC are the new concepts"
- "Redis and Postgres move inside Kubernetes"
- "local config first, then CI/CD, then cloud"
