---
title: The Deployment Process
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/59 - The Deployment Process.md
source_date: 2026-04-20
tags:
  - kubernetes
  - gke
  - production
  - deployment
  - autopilot
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/24 - The Path to Production.md
  - wiki/sources/60 - CI Setup.md
  - wiki/analysis/github-actions-gke-deployment-flow.md
---

# The Deployment Process

## Metadata

- Source type: deployment checklist note with one local screenshot and several externally hosted GCP UI screenshots
- Transcript quality: strong enough to preserve the production setup order, the GKE Autopilot creation path, and the cost warnings
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-20 at 3.13.33 PM.png]]
  - Additional UI screenshots are referenced externally by the source rather than stored in `raw/assets`
- Derived notes:
  - `derived/notes/59 - The Deployment Process - Cheat Sheet.md`
  - `derived/notes/59 - The Deployment Process - Explainer.md`

## Summary

This source turns the earlier high-level production path into a concrete checklist. The course's deployment story is: create a GitHub repo, connect it to CI, create a Google Cloud project, enable billing, add deployment scripts, and then create a GKE cluster. The source also updates the old course assumptions by steering the cluster-creation flow toward GKE Autopilot, which uses regions rather than zones and automatically manages node scaling.

## Key Claims

- Production deployment starts with version control and CI, not just with Kubernetes manifests.
- The course's checklist includes connecting the repo to Travis CI, because that is the CI system used in the course.
- A Google Cloud project must exist before the cluster can be created.
- Billing must be enabled for the Google Cloud project.
- The source now recommends GKE Autopilot as the cheaper and simpler cluster type for this workflow.
- Autopilot uses regions rather than zones.
- Autopilot automatically adds and removes nodes based on workload demand.
- Running the cluster costs real money and the source repeatedly warns the reader to shut it down when not using it.

## Important Evidence Or Examples

- The local screenshot summarizes the whole production checklist:
  - create GitHub repo
  - tie repo to Travis CI
  - create Google Cloud project
  - enable billing
  - add deployment scripts to the repo
- The numbered walkthrough then turns that checklist into concrete GKE UI steps:
  - open Kubernetes Engine > Clusters
  - enable the Kubernetes API
  - wait for container.googleapis.com to finish enabling
  - create a cluster
  - use the Autopilot option
  - name the cluster and create it
- The source explicitly warns that the cluster keeps billing while it is running.

## Important Commands

- No shell command is central in this source.
- The lesson is mainly a cloud-console workflow plus deployment planning checklist.

## Modern Equivalent

The source still says "Tie repo to Travis CI" because the course uses Travis.

For this vault, the modern equivalent is:

- create the GitHub repo
- use GitHub Actions instead of Travis CI
- connect GitHub Actions to Google Cloud
- build, test, push images, and deploy from the GitHub Actions workflow

The durable GitHub Actions version of that flow now lives in [[github-actions-gke-deployment-flow]].

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source is operationally useful, but much of the concrete cluster-creation walkthrough lives in external cloud UI screenshots rather than local canonical assets.
- The CI reference is historically specific to Travis CI, while the modern practical equivalent for this vault is GitHub Actions.
- The source warns about cloud cost clearly, but it does not yet connect that checklist to the exact GitHub Actions or deployment manifests that will run after cluster creation.
