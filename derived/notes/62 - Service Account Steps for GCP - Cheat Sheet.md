# Service Account Steps for GCP Cheat Sheet

## Source

- Transcript: `raw/transcripts/62 - Service Account Steps for GCP.md`
- Wiki source page: [[62 - Service Account Steps for GCP]]

## Old Course Flow

1. `IAM & Admin > Service Accounts`
2. create service account `travis-deployer`
3. grant `Kubernetes Engine Admin`
4. skip optional user grants
5. open `Manage Keys`
6. `Add Key > Create new key`
7. choose `JSON`
8. download the key file

## What That Key Was For

The downloaded file feeds the old Travis command:

```bash
gcloud auth activate-service-account --key-file service-account.json
```

## Modern Replacement

Preferred modern flow:

- `google-github-actions/auth`
- Workload Identity Federation
- `google-github-actions/get-gke-credentials`

JSON keys are now a fallback, not the preferred design.

## References

- `google-github-actions/auth`:
  https://github.com/google-github-actions/auth
- `google-github-actions/get-gke-credentials`:
  https://github.com/google-github-actions/get-gke-credentials
- full vault note:
  [[github-actions-gke-deployment-flow]]
