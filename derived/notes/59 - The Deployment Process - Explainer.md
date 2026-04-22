# The Deployment Process Explainer

## Source

- Transcript: `raw/transcripts/59 - The Deployment Process.md`
- Wiki source page: [[59 - The Deployment Process]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 3.13.33 PM.png]]

## What This Lesson Is Doing

This lesson is the production checklist that sits above all the individual Kubernetes objects.

Until now, the course has mostly been about manifests, services, deployments, ingress, secrets, and storage.

This source says:

all of that still needs a real deployment path.

## The Checklist

The picture is very simple:

1. create the GitHub repo
2. connect the repo to CI
3. create the Google Cloud project
4. enable billing
5. add deployment scripts
6. create the Kubernetes cluster

That is useful because it places Kubernetes into the larger delivery system instead of treating it like a standalone toy cluster.

## Why GKE Autopilot Matters

The updated part of the lesson is the GKE cluster choice.

It now recommends Autopilot.

That matters because:

- Autopilot is simpler than manually configured standard clusters
- Autopilot uses regions rather than zones
- Autopilot automatically scales the node layer

So the cluster-management burden is lower than it was in older GKE walkthroughs.

## The Real Lesson

The most durable takeaway is not the exact cloud-console clicks.

It is this:

- production deployment is not just writing YAML
- you also need repo hosting
- CI/CD
- a cloud project
- billing
- cluster lifecycle management

## What To Translate For Modern Use

The source still says "Tie repo to Travis CI" because that is what the course was built around.

For this vault, the better translation is:

- keep the checklist
- replace Travis CI with GitHub Actions

That modern mapping now lives in [[github-actions-gke-deployment-flow]].
