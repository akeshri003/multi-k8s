---
title: Ingress setup notes
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/53 - Ingress setup notes.md
source_date: 2026-04-20
tags:
  - kubernetes
  - ingress
  - ingress-nginx
  - googlecloud
  - localdev
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/52. - Quick Note on Ingress.md
  - wiki/sources/54 - Behind the Scenes of Ingress.md
---

# Ingress setup notes

## Metadata

- Source type: short transcript with one environment-warning diagram
- Transcript quality: brief but useful for preserving the setup caveat that ingress-nginx installation differs by environment
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-20 at 12.37.14 PM.png]]
- Derived notes:
  - `derived/notes/53 - Ingress setup notes - Cheat Sheet.md`
  - `derived/notes/53 - Ingress setup notes - Explainer.md`

## Summary

This source adds one practical warning before the ingress setup begins: ingress-nginx setup is environment-specific. Local setup, Google Cloud, AWS, and Azure can all require different steps. The course says it will cover local setup and later Google Cloud setup, and it hints that Google Cloud is being chosen over AWS for the production portion of this Kubernetes path.

## Key Claims

- Ingress-nginx setup is not uniform across environments.
- Local setup can differ significantly from cloud-provider setup.
- Google Cloud, AWS, Azure, and local environments may all require different ingress-nginx installation steps.
- The course will demonstrate ingress-nginx locally and later on Google Cloud.
- Environment-specific differences are a real source of ingress confusion.

## Important Evidence Or Examples

- The diagram explicitly states:
  - ingress-nginx setup changes depending on environment
  - the course will set it up locally and on Google Cloud
- The source frames this as a warning, not just background information.
- The transcript also foreshadows that the production deployment path in this course will use Google Cloud rather than AWS.

## Important Commands

- No shell command is central in this source.
- The lesson is a setup caveat rather than a procedural step.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source warns that ingress setup differs by environment, but it still does not show the local installation steps.
- The lesson foreshadows Google Cloud, but it does not yet explain why Google Cloud is preferred for the later production path.
- The practical local setup remains deferred to the next lessons.
