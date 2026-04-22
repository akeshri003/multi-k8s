# Triggering Deployment Updates Cheat Sheet

## Source

- Transcript: `raw/transcripts/22 - Triggering Deployment Updates.md`
- Wiki source page: [[22 - Triggering Deployment Updates]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 5.30.46 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 5.34.45 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 5.37.28 PM.png]]

## Core Problem

You built and pushed a newer image, but the deployment file did not change.

So this:

```bash
kubectl apply -f client-deployment.yaml
```

does not help if the file is unchanged.

Kubernetes treats it as unchanged instead of going to check Docker Hub for fresher image contents.

## Issue Reference

- Kubernetes GitHub issue `#33664`

## Three Workaround Families

1. delete pods manually and let the deployment recreate them
2. use real version tags like `v1`, `v2`, `v3` and update the config file
3. use real version tags and run an imperative command to update the deployment image

## Tradeoffs

- option 1:
  risky and clumsy
- option 2:
  more declarative, but adds an extra version-bump step to the deployment process
- option 3:
  still imperfect, but treated here as the most practical option

## Important Caveats

- environment variables are not presented as a usable easy fix inside these config files
- the exact imperative command is not shown yet in this lesson

## Retrieval Cues

- "`kubectl apply` only reacts to config changes"
- "registry change alone is invisible to unchanged manifest"
- "three bad-but-usable workaround families"
- "imperative image update is the next step"
