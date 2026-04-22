# The need for Volumes with Database Cheat Sheet

## Source

- Transcript: `raw/transcripts/37 - The need for Volumes with Database.md`
- Wiki source page: [[37 - The need for Volumes with Database]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 10.55.14 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 10.55.33 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 10.57.37 PM.png]]

## Core Idea

Databases cannot safely store important data only inside a container filesystem.

If the pod is replaced, that data disappears.

## The Problem

- Postgres writes data to disk
- if that disk is only the container filesystem
- then a crashed pod means lost data

## The Fix In Principle

- store the data outside the container
- let a replacement pod reconnect to the same storage

## Why This Matters For Postgres

- Postgres is not like Redis in-memory behavior
- it needs durable storage
- otherwise the database resets when the pod is recreated

## Important Warning

- do not just raise Postgres `replicas` to `2`
- two database copies blindly sharing the same storage is dangerous

## Retrieval Cues

- "container filesystem is not durable enough"
- "replacement pod should reconnect to the same data"
- "this is why Postgres needs persistence"
- "replicas are not the same thing as safe database HA"
