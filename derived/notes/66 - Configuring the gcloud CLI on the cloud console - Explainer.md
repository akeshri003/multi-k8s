# Configuring the gcloud CLI on the cloud console Explainer

## Source

- Transcript: `raw/transcripts/66 - Configuring the gcloud CLI on the cloud console.md`
- Wiki source page: [[66 - Configuring the gcloud CLI on the cloud console]]

## What This Lesson Is Actually About

This lesson is not mainly about `gcloud`.

It is really about environment boundaries.

Earlier in the course, the secret was created against the local Kubernetes environment.

But production is a different cluster.

So the secret has to exist there too.

## Why Cloud Shell Is Used

The course chooses Google Cloud Shell because it is already inside the project context.

That means it is a convenient place to do one-off admin work such as:

- targeting the right cluster
- running `kubectl`
- creating the missing secret

## The Three Configuration Steps

The lesson says to tell Cloud Shell:

1. which Google Cloud project to use
2. which compute zone the cluster lives in
3. which cluster credentials `kubectl` should fetch

That becomes:

```bash
gcloud config set project PROJECT_ID
gcloud config set compute zone COMPUTE_ZONE
gcloud container clusters get-credentials multi-cluster
```

## Mental Model

Think of this as "attaching your terminal to the right Kubernetes target."

Until you do that, a `kubectl create secret ...` command might:

- fail
- point nowhere useful
- or point at the wrong cluster

So this lesson is a prerequisite step for the real secret-creation command that comes next.

## Durable Takeaway

Even with a mostly declarative Kubernetes repo, there are still moments where a human operator has to target the live cluster and do one-time setup work carefully.
