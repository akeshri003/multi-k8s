# Persistent Volume vs Persistent Volume Claim Cheat Sheet

## Source

- Transcript: `raw/transcripts/40 - Persistent Volume vs Persistent Volume Claim.md`
- Wiki source page: [[40 - Persistent Volume vs Persistent Volume Claim]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 11.11.45 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.12.46 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.15.30 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.17.17 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.18.56 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.19.33 PM.png]]

## Core Idea

This lesson answers:

what is the difference between asking for storage and actually having storage?

## The Short Answer

- `PersistentVolumeClaim`
  - the request
  - the option you ask for
  - not the actual disk
- `PersistentVolume`
  - the real storage resource
  - the thing that actually stores data

## Course Analogy

- billboard = storage options being advertised
- customer = developer / pod config asking for storage
- salesperson = Kubernetes
- hard drive already in stock = statically provisioned `PersistentVolume`
- hard drive made on demand = dynamically provisioned `PersistentVolume`

## What Kubernetes Does

When your pod needs storage:

1. you make a `PersistentVolumeClaim`
2. Kubernetes checks whether matching persistent storage already exists
3. if it exists, Kubernetes binds you to it
4. if it does not exist, Kubernetes may create it on demand

## Retrieval Cues

- "PVC asks"
- "PV stores"
- "claim is request, volume is resource"
- "static means pre-created"
- "dynamic means created when requested"
