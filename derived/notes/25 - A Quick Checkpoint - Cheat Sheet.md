# A Quick Checkpoint Cheat Sheet

## Source

- Transcript: `raw/transcripts/25 - A Quick Checkpoint.md`
- Wiki source page: [[25 - A Quick Checkpoint]]

## Core Idea

Before migrating the `complex` app into Kubernetes, make sure the original Docker Compose version still works.

If the old app is already broken, the Kubernetes migration will be much harder to debug.

## Main Commands

```bash
docker ps
docker-compose up --build
docker-compose up
```

## What To Check First

- make sure Docker is pointing at your normal local Docker daemon
- do not run Docker Compose against the Docker daemon inside the Kubernetes node
- if needed, open a fresh terminal window to reset that context

## What To Check In The App

- open:

```text
localhost:3050
```

- confirm the existing Fibonacci app loads
- submit a new value like `12`
- confirm the submitted index appears
- confirm the calculated value also appears

## Backup Advice

- if your `complex` project may have drifted, use the provided zip backup as the clean starting point

## Why This Lesson Matters

- this is a sanity check before the Kubernetes migration
- it confirms the app codebase is still healthy
- it reduces the risk of blaming Kubernetes for problems that already existed

## Retrieval Cues

- "verify baseline app before migration"
- "docker context must be local, not node Docker"
- "`docker-compose up --build` first"
- "`localhost:3050` must still work"
