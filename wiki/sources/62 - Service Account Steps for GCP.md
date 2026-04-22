---
title: Service Account Steps for GCP
type: source
status: active
created: 2026-04-20
updated: 2026-04-20
source_path: raw/transcripts/62 - Service Account Steps for GCP.md
source_date: 2026-04-20
tags:
  - kubernetes
  - ci
  - cd
  - gcp
  - service-account
  - json-key
  - github-actions
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/61 - Installing google cloud sdk.md
  - wiki/analysis/github-actions-gke-deployment-flow.md
---

# Service Account Steps for GCP

## Metadata

- Source type: short GCP UI walkthrough with externally hosted screenshots
- Transcript quality: strong enough to preserve the old service-account creation flow and the exact key-generation intent
- Embedded visuals:
  - The walkthrough references multiple external GCP UI screenshots, but no matching local assets were added in `raw/assets`
- Derived notes:
  - `derived/notes/62 - Service Account Steps for GCP - Cheat Sheet.md`
  - `derived/notes/62 - Service Account Steps for GCP - Explainer.md`

## Summary

This source is the old Google Cloud companion step for the Travis-based CI setup. It walks through creating a service account in GCP, granting it `Kubernetes Engine Admin`, and generating a JSON key file so the Travis pipeline can authenticate with `gcloud auth activate-service-account --key-file service-account.json`. The durable modern interpretation is that this is an old key-file based authentication flow; for this vault, GitHub Actions with Workload Identity Federation is the preferred replacement, while JSON keys should be treated as a fallback only.

## Key Claims

- The walkthrough exists because the GCP UI had changed from the original course screenshots.
- The service account should be created from `IAM & Admin > Service Accounts`.
- The service-account name used in the course is `travis-deployer`.
- The course assigns the `Kubernetes Engine Admin` role to that account.
- The "Grant users access" step is optional and should be skipped.
- The course then opens `Manage Keys`, creates a new key, chooses JSON, and downloads the key file.
- That JSON file is the credential that will later be used by the old Travis pipeline.

## Important Evidence Or Examples

The walkthrough sequence is:

1. open `IAM & Admin > Service Accounts`
2. create the service account
3. choose `Kubernetes Engine > Kubernetes Engine Admin`
4. skip optional user-access grants
5. open `Manage Keys`
6. choose `Add Key > Create new key`
7. choose `JSON`
8. download the JSON key file

The transcript explicitly ties this output to the older CI authentication model from the surrounding lessons.

## Important Commands

The source itself is a cloud-console walkthrough, but it clearly feeds into the older command from the previous lesson:

```bash
gcloud auth activate-service-account --key-file service-account.json
```

## Modern Equivalent

For this vault, the preferred modern replacement is:

- GitHub Actions
- `google-github-actions/auth`
- Workload Identity Federation where possible
- `google-github-actions/get-gke-credentials` for cluster access

The official `google-github-actions/auth` documentation says Workload Identity Federation is recommended over service-account keys.

If a JSON key is still used as a fallback, it should be treated as a high-risk secret and not as the first-choice modern design.

## Modern Notes With References

- `google-github-actions/auth` documents both Workload Identity Federation and `credentials_json`, and recommends federation over keys:
  https://github.com/google-github-actions/auth
- `google-github-actions/get-gke-credentials` is the official GitHub Action for configuring GKE access for `kubectl`:
  https://github.com/google-github-actions/get-gke-credentials
- The full durable modern flow for this vault is tracked in [[github-actions-gke-deployment-flow]].

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The walkthrough is operationally useful for understanding the old course flow, but it is still a long-lived-key workflow rather than the modern preferred auth model.
- The source references multiple UI screenshots externally, but those images are not yet stored in `raw/assets` as local canonical evidence.
- The assigned role is intentionally broad for course simplicity, but a future vault note should tighten this into the minimum Google Cloud IAM permissions needed for the GitHub Actions workflow.
