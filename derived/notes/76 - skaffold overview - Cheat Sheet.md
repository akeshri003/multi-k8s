# skaffold overview Cheat Sheet

## Source

- Transcript: `raw/transcripts/76 - skaffold overview.md`
- Wiki source page: [[76 - skaffold overview]]

## Key Visual

- ![[raw/assets/Pasted image 20260423010827.png]]

## Problem

- Kubernetes local dev is slow if every code change means:
  - rebuild image
  - reapply manifests

## Skaffold Modes

1. rebuild image and redeploy
2. sync changed files into running container

## Best Fit

- mode two works well with Create React App and Nodemon
