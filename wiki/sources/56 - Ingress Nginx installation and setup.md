---
title: Ingress Nginx installation and setup
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/56 - Ingress Nginx installation and setup.md
source_date: 2026-04-20
tags:
  - kubernetes
  - ingress
  - ingress-nginx
  - docker-desktop
  - setup
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/52. - Quick Note on Ingress.md
  - wiki/sources/53 - Ingress setup notes.md
  - wiki/sources/57 - Creating Ingress Configuration.md
---

# Ingress Nginx installation and setup

## Metadata

- Source type: short setup note with an external documentation link and one externally hosted screenshot
- Transcript quality: brief but strong enough to preserve the Docker Desktop-specific installation pointer and the exact documentation section the lesson wants the user to follow
- Embedded visuals:
  - External image referenced directly by the source:
    ![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2021-12-17_19-05-02-747c83600cdba303ec7b8a002e523251.png)
- Derived notes:
  - `derived/notes/56 - Ingress Nginx installation and setup - Cheat Sheet.md`
  - `derived/notes/56 - Ingress Nginx installation and setup - Explainer.md`

## Summary

This source is a short operational bridge between the ingress architecture lessons and the first real ingress manifest. Instead of spelling out the full install command locally, it tells Docker Desktop Kubernetes users to use the official ingress-nginx quick-start documentation, specifically the "If you don't Have Helm" section, and run the provided `kubectl apply` script from there.

## Key Claims

- The local ingress-nginx installation path depends on the local environment.
- For Docker Desktop Kubernetes on macOS or Windows, the lesson points readers to the official ingress-nginx quick-start docs.
- The intended documentation page is `https://kubernetes.github.io/ingress-nginx/deploy/#quick-start`.
- The lesson specifically tells the reader to use the "If you don't Have Helm" section.
- The install step is still a `kubectl apply` workflow, even though the exact manifest lives in the official docs rather than in the course repo.

## Important Evidence Or Examples

- The source explicitly names Docker Desktop's Kubernetes as the target local environment.
- The source repeats the same quick-start URL twice, which makes the documentation link the main durable artifact of the lesson.
- The source does not paste the actual install command into the transcript; it only instructs the reader to copy the script from the docs page.

## Important Commands

- The exact shell command is not transcribed locally.
- The source instructs the reader to copy and run the `kubectl apply` script from:
  - `https://kubernetes.github.io/ingress-nginx/deploy/#quick-start`
  - specifically the "If you don't Have Helm" section

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The install command is not preserved directly in the local source, so the lesson depends on external docs for the exact manifest URL and `kubectl apply` command.
- The source is Docker Desktop-specific, so it should not be treated as universal ingress-nginx setup guidance for all local or cloud environments.
- The transcript includes an external image URL rather than a local asset in `raw/assets`, so this visual is not yet part of the vault's canonical asset store.
