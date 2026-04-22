# Rebuilding the Client Image Cheat Sheet

## Source

- Transcript: `raw/transcripts/21 - Rebuilding the Client Image.md`
- Wiki source page: [[21 - Rebuilding the Client Image]]

## Core Idea

This lesson creates a visibly newer `multi-client` image.

The source code change is simple:

```text
Fib calculator
```

becomes:

```text
Fib calculator version two
```

## Main Commands

```bash
docker build -t <docker-id>/multi-client .
docker push <docker-id>/multi-client
```

## What Actually Happens

- edit `App.js`
- rebuild the Docker image
- push the rebuilt image to Docker Hub
- Docker Hub now has the latest image
- Kubernetes still has not been told how to move the deployment onto it

## Retrieval Cues

- "change browser text so image version is obvious"
- "rebuild multi-client image"
- "push latest image to Docker Hub"
- "registry update alone is not enough"
