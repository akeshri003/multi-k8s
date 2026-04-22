---
title: Recreating the Deployment
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/26 - Recreating the Deployment.md
source_date: 2026-04-19
tags:
  - kubernetes
  - deployment
  - migration
  - manifests
  - multi-client
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/24 - The Path to Production.md
  - wiki/sources/25 - A Quick Checkpoint.md
---

# Recreating the Deployment

## Metadata

- Source type: transcript with inline YAML example
- Transcript quality: strong enough to preserve the project cleanup, the new `k8s/` folder setup, and the first real migration manifest
- Embedded visuals:
  - None.
- Derived notes:
  - `derived/notes/26 - Recreating the Deployment - Cheat Sheet.md`
  - `derived/notes/26 - Recreating the Deployment - Explainer.md`

## Summary

This source begins the actual migration from the old Docker Compose and Elastic Beanstalk project into Kubernetes manifests. It first cleans up the `complex` project by removing files and folders that are no longer needed for the Kubernetes version, then creates a new `k8s/` directory to hold the manifest files. The lesson's first concrete manifest is `client-deployment.yml`, which recreates the `multi-client` frontend as a Kubernetes `Deployment` with three replicas, the `component: web` label/selector pair, and a single `client` container exposing port `3000`.

## Key Claims

- Once the Kubernetes migration starts, the project directory should be cleaned of old deployment-specific files that no longer fit the new workflow.
- The course deletes the old Travis, Docker Compose, and Docker-run-related files because Kubernetes will replace those environments.
- The old Nginx folder is also removed because routing will now be handled by `Ingress` rather than the earlier custom Nginx image.
- A dedicated `k8s/` directory is created to hold the Kubernetes configuration files.
- The course expects roughly eleven config files for the full app architecture.
- The first new manifest is the `multi-client` deployment.
- That deployment is configured to manage three frontend pods.
- The label/selector pair still uses `component: web`, carrying over the earlier mental model from the small Kubernetes examples.
- The `multi-client` container still uses port `3000` inside the pod template.

## Important Evidence Or Examples

- The cleanup step removes old project artifacts tied to the previous deployment paths:
  - `.travis.yml`
  - the Docker Compose file
  - the Docker-run-related file(s)
  - the `nginx/` folder
- The new manifest folder is:

```text
k8s/
```

- The new deployment file is:

```text
k8s/client-deployment.yml
```

- The transcript's deployment example is:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: client-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      component: web
  template:
    metadata:
      labels:
        component: web
    spec:
      containers:
        - name: client
          image: stephengrider/multi-client
          ports:
            - containerPort: 3000
```

- The lesson explains the structure in practical terms:
  - `apiVersion` and `kind` declare that this file creates a deployment
  - `metadata.name` names it `client-deployment`
  - `replicas: 3` requests three pods
  - `selector.matchLabels` and `template.metadata.labels` line up through `component: web`
  - the pod template contains one `client` container using the `multi-client` image on port `3000`

## Important Commands

- No shell command is central in this source.
- The main operational action is creating and editing:

```text
k8s/client-deployment.yml
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson starts deleting old deployment files before the new Kubernetes replacements are fully built, so the migration now assumes the old stack has already served its purpose as a reference point.
- The transcript's pasted YAML appears lightly malformed in places, especially around indentation and `label` versus `labels`, so the normalized version above follows the spoken explanation and the broader lesson sequence rather than the raw paste exactly.
- The source builds the `client` deployment first, but it postpones the next step of creating the matching service and explaining `ClusterIP` in detail.
