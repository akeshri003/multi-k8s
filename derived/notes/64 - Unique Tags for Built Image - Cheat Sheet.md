# Unique Tags for Built Image Cheat Sheet

## Source

- Transcript: `raw/transcripts/64 - Unique Tags for Built Image.md`
- Wiki source page: [[64 - Unique Tags for Built Image]]

## Core Fix

- every build needs a unique tag
- the course uses the current Git commit SHA

## Key Commands

```bash
git rev-parse HEAD
git log
```

## Tag Strategy

- `DOCKER_ID/multi-client:latest`
- `DOCKER_ID/multi-client:$SHA`

Repeat the same pattern for `multi-server` and `multi-worker`.

## Why Keep `latest`

- fresh `kubectl apply -f k8s` still gets the newest default images

## Why Use `$SHA`

- deployment updates become deterministic
- debugging gets easier because image version maps back to an exact commit

See also: [[65 - Updating the Deployment Script]]
