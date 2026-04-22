# Configuring the gcloud CLI on the cloud console Cheat Sheet

## Source

- Transcript: `raw/transcripts/66 - Configuring the gcloud CLI on the cloud console.md`
- Wiki source page: [[66 - Configuring the gcloud CLI on the cloud console]]

## Goal

- prepare Google Cloud Shell so `kubectl` talks to the remote GKE cluster

## Why

- the production cluster still needs the `pgpassword` secret created remotely

## Commands

```bash
gcloud config set project PROJECT_ID
gcloud config set compute zone COMPUTE_ZONE
gcloud container clusters get-credentials multi-cluster
```

## Durable Idea

- local cluster setup is not enough
- some cluster-scoped setup must be repeated against the remote cluster

See also: [[48 - Creating an Encoded Secret]]
