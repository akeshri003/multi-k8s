# Defining Environment Variables Cheat Sheet

## Source

- Transcript: `raw/transcripts/46 -Defining Environment Variables.md`
- Wiki source page: [[46 -Defining Environment Variables]]

## Key Visuals

- ![[Screenshot 2026-04-20 at 10.55.23 AM.png]]
- ![[Screenshot 2026-04-20 at 10.55.58 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 10.57.50 AM.png]]

## Core Idea

This lesson starts the runtime wiring phase.

It says the environment variables fall into two broad groups.

## Two Groups

- ordinary constant values
- connection-style values that behave like addresses for reaching other services

## What The Colors Mean

- yellow = constant values
- red = constant-looking values that are really used to establish connections

## Why This Matters

- Redis and Postgres cannot be used just because their Kubernetes objects exist
- the app containers still need the right connection-related environment variables
- `redis-cluster-ip-service` is an example of the connection-style group, because the worker uses it to reach Redis through the cluster service layer

## Retrieval Cues

- "env vars are the next step"
- "yellow = constants"
- "red = connection values"
