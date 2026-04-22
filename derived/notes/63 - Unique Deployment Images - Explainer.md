# Unique Deployment Images Explainer

## Source

- Transcript: `raw/transcripts/63 - Unique Deployment Images.md`
- Wiki source page: [[63 - Unique Deployment Images]]

## What This Lesson Is Doing

This lesson writes the first believable production deploy script.

The script does the right categories of work:

1. build production images
2. push them to Docker Hub
3. apply the Kubernetes manifests
4. point deployments at the intended images

That sounds correct, but the lesson deliberately pauses to show that the image-reference part is still flawed.

## Why The First Draft Still Fails

Kubernetes deployments react to changes in the image reference they are told to use.

If the script keeps pointing the deployment at the same mutable image name, the deployment can decide:

- nothing changed
- no rollout is needed

That is the same image-freshness problem the course already exposed earlier.

So the important takeaway is not just the command list.

It is this operational insight:

- a deploy script can automate the right workflow stages
- and still fail if image identity is not explicit enough

## Mental Model

Think of `latest` as "whatever the registry currently says is the default."

That is convenient for humans.

It is not good enough as a precise deployment version.

Production rollout logic wants something closer to:

- "use exactly this build artifact"

That is what the next lesson fixes with Git SHAs.

## Why This Matters For Future Notes

This is where the course stops being "apply YAML and hope" and starts becoming a real deployment pipeline.

The main progression is:

- manifests describe the app
- CI builds the images
- the deploy script applies config
- the deploy script must also choose exact image versions

See also: [[64 - Unique Tags for Built Image]] and [[65 - Updating the Deployment Script]]
