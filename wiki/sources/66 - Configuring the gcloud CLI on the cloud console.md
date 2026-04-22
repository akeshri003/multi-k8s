---
title: Configuring the gcloud CLI on the cloud console
type: source
status: active
created: 2026-04-22
updated: 2026-04-22
source_path: raw/transcripts/66 - Configuring the gcloud CLI on the cloud console.md
source_date: 2026-04-22
tags:
  - kubernetes
  - gcp
  - gke
  - gcloud
  - cloud-shell
  - secret
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/48 - Creating an Encoded Secret.md
  - wiki/sources/62 - Service Account Steps for GCP.md
  - wiki/sources/65 - Updating the Deployment Script.md
  - wiki/analysis/multi-k8s-google-cloud-ui-setup-for-github-actions.md
---

# Configuring the gcloud CLI on the cloud console

## Metadata

- Source type: transcript with no locally embedded screenshots
- Transcript quality: strong enough to preserve the cluster-shell setup sequence and the reason a remote secret still has to be created manually
- Embedded visuals:
  - No local screenshots are embedded in this source
- Derived notes:
  - `derived/notes/66 - Configuring the gcloud CLI on the cloud console - Cheat Sheet.md`
  - `derived/notes/66 - Configuring the gcloud CLI on the cloud console - Explainer.md`

## Summary

This source explains the last manual Google Cloud setup step before the first production deploy attempt. The cluster still needs the `pgpassword` Kubernetes secret to exist remotely, and the course chooses Google Cloud Shell as the easiest place to run that cluster-scoped `kubectl` command. Before the secret can be created, Cloud Shell has to be pointed at the correct project, compute zone, and cluster with the same `gcloud` and credential commands that appeared earlier in the CI setup.

## Key Claims

- The production cluster does not automatically inherit the Kubernetes secret that was created locally.
- The `pgpassword` secret still needs to be created imperatively on the remote cluster.
- Google Cloud Shell is presented as the easiest way to run `kubectl` against the GKE cluster from inside the Google Cloud project context.
- Before using `kubectl`, Cloud Shell must be configured with:
  - the project ID
  - the compute zone
  - cluster credentials
- This setup is usually a one-time step per project and cluster.

## Important Evidence Or Examples

The transcript maps the Cloud Shell configuration directly to the earlier CI commands:

```bash
gcloud config set project PROJECT_ID
gcloud config set compute zone COMPUTE_ZONE
gcloud container clusters get-credentials multi-cluster
```

The whole point of that setup is to make the next imperative cluster-secret command possible.

The source explicitly ties this back to the earlier `pgpassword` secret used by:

- the `multi-server` deployment
- the Postgres deployment

## Important Commands

```bash
gcloud config set project PROJECT_ID
gcloud config set compute zone COMPUTE_ZONE
gcloud container clusters get-credentials multi-cluster
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source is another reminder that the overall deployment flow is not purely declarative, because the remote cluster still needs one-off imperative setup.
- The exact follow-up secret-creation command is deferred to the next lesson, so this source mainly prepares the shell context rather than completing the task itself.
- The cluster name, project ID, and compute zone are environment-specific, so the commands are durable as a pattern but not portable as literals.
