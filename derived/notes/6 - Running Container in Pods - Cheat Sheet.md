# Running Container in Pods Cheat Sheet

## Source

- Transcript: `raw/transcripts/6 - Running Container in Pods.md`
- Wiki source page: [[6 - Running Container in Pods]]
- Key visuals:
  - ![[raw/assets/Screenshot 2026-04-17 at 7.48.36 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 8.43.26 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 8.45.37 PM.png]]

## Core Idea

In Kubernetes, the smallest deployable unit is a `Pod`, not a naked container.

## What A Pod Is

- A pod runs one or more containers.
- The point of a pod is to group containers with a very tight relationship.
- In this course, the first examples use one container per pod.

## When Multiple Containers In One Pod Make Sense

- only when the containers have to live together
- only when they are tightly coupled
- only when one container becomes useless if the primary container disappears

Example from the lesson:

- primary `postgres` container
- support `logger` container
- support `backup-manager` container

## What The Course Is Saying Not To Do

Do not think of a pod as "just cram the whole multi-container app into one box."

The lesson's point is:

- the earlier app's containers were related
- but most of them could still function independently enough to avoid being one pod

## Retrieval Cues

- "smallest deployable unit is a pod"
- "pod groups tightly coupled containers"
- "postgres plus logger plus backup-manager example"
