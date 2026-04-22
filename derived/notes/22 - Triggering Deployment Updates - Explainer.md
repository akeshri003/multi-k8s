# Triggering Deployment Updates Explainer

## Source

- Transcript: `raw/transcripts/22 - Triggering Deployment Updates.md`
- Wiki source page: [[22 - Triggering Deployment Updates]]

## What Question This Lesson Answers

By this point in the sequence, you already did two things:

- rebuilt the `multi-client` image
- pushed that image to Docker Hub

So the natural question is:

why not just run `kubectl apply -f` again and let Kubernetes figure it out?

This lesson explains why that does not work well.

## The Core Problem

Kubernetes updates through config changes.

So when you run:

```bash
kubectl apply -f client-deployment.yaml
```

Kubernetes compares the file you are applying to the last applied config.

If the file text is unchanged, Kubernetes treats it as unchanged.

The important consequence is:

it does not say, "let me check Docker Hub to see whether the image behind this same tag has newer contents now."

That is why this feels awkward.

The registry changed, but the manifest did not.

## The GitHub Issue

The lesson points to Kubernetes issue:

```text
#33664
```

That is used here as evidence that this is a real ecosystem problem, not just a confusion in the course.

## Workaround 1: Delete Pods Manually

The first idea is:

- delete the pods
- let the deployment notice they are missing
- let the deployment recreate them

The hope is that the recreated pods would pull the latest image.

The course dislikes this option for good reason:

- it is easy to delete the wrong pods
- it creates unnecessary risk
- it can create a brief outage window

This is why the note and diagram call it a bad idea.

## Workaround 2: Use Real Version Tags In The Config File

The second idea is cleaner in principle.

Instead of reusing one image reference, build versioned images such as:

```text
multi-client:v1
multi-client:v2
multi-client:v3
```

Then update the deployment file to point at the new tag.

That works because changing the image tag is a real manifest change, so `kubectl apply` now has something meaningful to process.

The downside is workflow cost.

Now every deployment update means:

- build a new versioned image
- push it
- update the config file
- keep the config and build version in sync

The lesson argues that this becomes especially annoying in CI because build timing and config updates have to be coordinated carefully.

## Workaround 3: Use A Versioned Image Plus An Imperative Update Command

The third idea still uses real image versions.

But instead of editing the config file each time, you run a command after the build that tells Kubernetes to update the deployment to the new version.

That means:

- you still benefit from explicit version tags
- you avoid constant config-file churn
- but you accept an imperative step

The course says this is not ideal, but it is the most practical of the bad options presented here.

That is an important nuance:

the course still prefers declarative workflows in general, but it is willing to accept an imperative deployment-image update when the alternative is worse operational friction.

## Why This Lesson Matters

This lesson explains a very common confusion:

"I pushed a new image, so why didn't my deployment magically update?"

The answer is:

because Kubernetes mostly reacts to declared desired state, not to invisible registry-side content changes hiding behind the same image reference.

That is why image versioning and rollout triggering become their own operational concern.

## Best Reuse

Use this note when you want a simple explanation of:

- why unchanged manifests do not refresh images
- why this image-update problem exists at all
- the three workaround families the course compares
- why the course reluctantly accepts an imperative command as the most workable next step
