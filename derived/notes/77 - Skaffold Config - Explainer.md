# Skaffold Config Explainer

## Source

- Transcript: `raw/transcripts/77 - Skaffold Config.md`
- Wiki source page: [[77 - Skaffold Config]]

## Key Visual

- ![[raw/assets/Pasted image 20260423011457.png]]

## The Big Idea

This is not just a Skaffold syntax lesson.

It is a separation-of-concerns lesson.

Once the repo has real production HTTPS resources, local development can no longer casually reuse the exact same ingress and certificate files.

So the repo gets split into:

- shared base app manifests
- local-dev extras
- production-only TLS extras

## Why `k8s-dev` And `k8s-prod` Matter

Production ingress, certificate, and issuer manifests are tied to a real domain and certificate flow.

Those do not belong in the local inner loop.

Skaffold should instead deploy:

- the base workloads and services
- a dev-friendly ingress manifest

That is the clean boundary this source preserves.

## Why The Dockerfile Vars Matter

`CI=true` and `WDS_SOCKET_PORT=0` are not random tweaks.

They are there to make the React dev server behave more reliably when running behind this Kubernetes + Skaffold setup.

So this source is partly about folder structure and partly about making the dev-server feedback loop actually work.
