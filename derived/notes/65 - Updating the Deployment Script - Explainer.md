# Updating the Deployment Script Explainer

## Source

- Transcript: `raw/transcripts/65 - Updating the Deployment Script.md`
- Wiki source page: [[65 - Updating the Deployment Script]]

## What Changed In The Pipeline

The previous lesson gave the image-tagging strategy.

This lesson wires that strategy into the CI and deploy files.

Two things happen:

1. CI exports the current commit SHA into an environment variable
2. the deploy script starts building, pushing, and deploying SHA-tagged images

## Why `CLOUDSDK_CORE_DISABLE_PROMPTS=1` Matters

This is a small but important CI hardening step.

Interactive prompts are fine on a human terminal.

They are bad in CI because the job can stall waiting for input that will never come.

So this env var is basically telling the Google Cloud CLI:

- do not ask questions
- run non-interactively

## Why The Build Step Doubles The Tags

Each image now gets:

- `latest`
- `$SHA`

That looks redundant at first, but it separates two use cases:

- `latest` keeps the repository easy to apply from scratch
- `$SHA` gives production an immutable rollout target

## Why The Push Step Also Doubles

Docker pushes tags, not abstract image concepts.

So once one local image has two tags, both tags have to be pushed separately.

That is why the script gets longer.

## Why The Final `kubectl set image` Commands Use `$SHA`

This is the real deployment step.

The script is no longer saying:

- "run whatever is currently latest"

It is saying:

- "run this exact image version that came from this exact commit"

That is what makes the rollout deterministic.

## Modern Translation

In GitHub Actions, the same idea usually becomes:

- `GITHUB_SHA`
- build and push `${GITHUB_SHA}` tags
- update deployments to those exact tags

So the course is still teaching a durable deployment pattern even though the CI platform is old.
