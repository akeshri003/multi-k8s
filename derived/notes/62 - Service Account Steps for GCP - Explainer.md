# Service Account Steps for GCP Explainer

## Source

- Transcript: `raw/transcripts/62 - Service Account Steps for GCP.md`
- Wiki source page: [[62 - Service Account Steps for GCP]]

## What This Lesson Is Doing

This lesson is the Google Cloud side of the old Travis authentication model.

The previous lesson showed the command:

```bash
gcloud auth activate-service-account --key-file service-account.json
```

This lesson explains where that `service-account.json` file came from.

## The Old Course Flow

The source walks through:

1. creating a service account in GCP
2. granting it `Kubernetes Engine Admin`
3. generating a JSON key
4. downloading that key for use in CI

So the course's old auth model was:

- create a service account
- download a long-lived JSON credential
- give that file to Travis
- let Travis authenticate with `gcloud`

## Why This Worked Then

This was a common CI pattern in older cloud tutorials.

It was practical and easy to explain:

- make an account
- give it a role
- generate a key
- use that key in CI

## Why The Modern Recommendation Is Different

The official `google-github-actions/auth` documentation says Workload Identity Federation is recommended over service-account keys.

That matters because a JSON key is long-lived.

If it leaks, it can be abused until it is rotated or revoked.

So the modern preferred model is:

- GitHub Actions OIDC token
- short-lived Google Cloud credentials
- no default dependence on a static JSON key file

## Practical Modern Translation

For this vault, the old course logic becomes:

- keep the idea that CI needs Google Cloud identity
- replace downloaded JSON keys with GitHub Actions federation when possible
- use `get-gke-credentials` to configure `kubectl`

That gives you the same deployment capability with a cleaner and safer auth path.

## When A JSON Key Still Appears

A JSON key can still be used as a fallback.

The official `auth` action supports `credentials_json`.

But it should be treated as:

- a fallback option
- a high-sensitivity secret
- something to minimize and eventually replace

## References

- `google-github-actions/auth`:
  https://github.com/google-github-actions/auth
- `google-github-actions/get-gke-credentials`:
  https://github.com/google-github-actions/get-gke-credentials
- full vault modernization note:
  [[github-actions-gke-deployment-flow]]
