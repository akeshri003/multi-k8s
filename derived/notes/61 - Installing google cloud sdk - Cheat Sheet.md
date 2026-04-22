# Installing google cloud sdk Cheat Sheet

## Source

- Transcript: `raw/transcripts/61 - Installing google cloud sdk.md`
- Wiki source page: [[61 - Installing google cloud sdk]]

## Old Course Travis Block

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

## What It Was Doing

- install Google Cloud SDK
- load SDK shell config
- install `kubectl`
- authenticate to Google Cloud with a service-account key file

## Modern GitHub Actions Equivalent

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

## Why The Modern Version Is Better

- no manual SDK bootstrap script
- no default reliance on long-lived `service-account.json`
- GKE credentials are configured through the official action

## References

- `google-github-actions/auth`:
  https://github.com/google-github-actions/auth
- `google-github-actions/get-gke-credentials`:
  https://github.com/google-github-actions/get-gke-credentials
- full vault note:
  [[github-actions-gke-deployment-flow]]
