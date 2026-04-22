# Unique Tags for Built Image Explainer

## Source

- Transcript: `raw/transcripts/64 - Unique Tags for Built Image.md`
- Wiki source page: [[64 - Unique Tags for Built Image]]

## The Real Problem This Solves

Earlier in the course, the deployment-update story was:

- build a newer image
- push it
- tell the deployment to use it

But Kubernetes needs to see a new image reference, not just newer bytes hiding behind the same name.

So the course needs a version identifier that is:

- unique per build
- automatic
- easy to trace back to source code

The Git commit SHA fits all three.

## Why Git SHA Is Better Than Hand-Made Version Numbers

You could manually write `v2`, `v3`, `v4`, and so on.

That works technically, but it creates unnecessary human bookkeeping.

A Git SHA gives you:

- uniqueness
- automation
- traceability to the exact code snapshot

## Debugging Benefit

This is the most durable insight from the lesson.

If production breaks and the deployment is running an image tagged with a commit SHA, you can:

1. inspect the deployed image tag
2. copy the SHA
3. `git checkout` that commit locally
4. debug the exact code version that production is running

That is much stronger than trying to reason about a vague mutable `latest` image.

## Why The Course Still Keeps `latest`

The course wants both convenience and determinism.

`latest` stays useful for:

- a new engineer cloning the repo
- applying the manifests into a fresh cluster
- pulling the current default image without extra setup

So the split becomes:

- `latest` for default fresh-start behavior
- `$SHA` for exact production rollout behavior

That same pattern maps directly onto the vault’s GitHub Actions notes via `GITHUB_SHA`.
