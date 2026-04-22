# Installing google cloud sdk Explainer

## Source

- Transcript: `raw/transcripts/61 - Installing google cloud sdk.md`
- Wiki source page: [[61 - Installing google cloud sdk]]

## What This Lesson Is Doing

This lesson is the first concrete CI bootstrap step.

The course is still using Travis CI, so it shows how Travis prepares itself to talk to Google Cloud and Kubernetes before any deployment happens.

## The Old Course Approach

The transcript's Travis block is:

```yaml
sudo: required

services:
  - docker

before_install:
  - curl https://sdk.cloud.google.com | bash > /dev/null;
  - source $HOME/google-cloud-sdk/path.bash.inc
  - gcloud components update kubectl
  - gcloud auth activate-service-account --key-file service-account.json
```

That means:

1. install the Google Cloud SDK inside the CI runner
2. load the shell configuration it created
3. install `kubectl`
4. authenticate to Google Cloud using a service-account key file

That was a reasonable pattern for older Travis-based pipelines.

## The Security Boundary

The transcript correctly calls out the most sensitive part:

```text
service-account.json
```

That file is a long-lived credential.

If it leaks, someone may be able to access your Google Cloud resources.

So even in the old course flow, the speaker treats it as highly sensitive.

## The Modern GitHub Actions Replacement

For a modern GitHub Actions pipeline, the same underlying need still exists:

- authenticate to Google Cloud
- get cluster credentials
- let `kubectl` talk to GKE

But the implementation is cleaner:

```yaml
- uses: actions/checkout@v4

- uses: google-github-actions/auth@v3
  with:
    project_id: YOUR_PROJECT_ID
    workload_identity_provider: YOUR_WORKLOAD_IDENTITY_PROVIDER
    service_account: YOUR_SERVICE_ACCOUNT

- uses: google-github-actions/get-gke-credentials@v3
  with:
    cluster_name: YOUR_CLUSTER_NAME
    location: YOUR_CLUSTER_LOCATION
```

## Why This Modern Flow Is Better

### 1. No Manual SDK Install Script

You do not need to `curl`-install the SDK yourself in the same way the old Travis flow did.

### 2. Better Authentication Model

The official `google-github-actions/auth` documentation says Workload Identity Federation is recommended over service-account keys.

That means the preferred modern setup is:

- GitHub Actions OIDC
- short-lived Google Cloud credentials
- less dependence on a static JSON key file

### 3. Cleaner GKE Access

The official `get-gke-credentials` action exists specifically to set up GKE access for `kubectl`.

That makes the workflow easier to read and easier to maintain than hand-written shell bootstrap logic.

## Practical Translation

So the safe mental model is:

- keep the course lesson for understanding why CI needs cloud auth and `kubectl`
- replace the old Travis bootstrap with the official GitHub Actions auth and GKE-credentials actions

## References

- `google-github-actions/auth`:
  https://github.com/google-github-actions/auth
- `google-github-actions/get-gke-credentials`:
  https://github.com/google-github-actions/get-gke-credentials
- full vault modernization note:
  [[github-actions-gke-deployment-flow]]
