# The Deployment Process Cheat Sheet

## Source

- Transcript: `raw/transcripts/59 - The Deployment Process.md`
- Wiki source page: [[59 - The Deployment Process]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 3.13.33 PM.png]]

## Core Idea

Before the app can be deployed to Kubernetes in production, the course wants this setup sequence:

- create the GitHub repo
- connect the repo to CI
- create the Google Cloud project
- enable billing
- add deployment scripts
- create the GKE cluster

## GKE Cluster Choice

- use GKE Autopilot
- Autopilot uses regions, not zones
- Autopilot automatically manages nodes for you

## Important Warning

- the cluster costs real money while it is running
- shut it down when you are not using it

## Modern Equivalent

The course says "Tie repo to Travis CI."

For this vault, the modern equivalent is:

- tie the repo to GitHub Actions

See also: [[github-actions-gke-deployment-flow]]
