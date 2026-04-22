# Where does Kubernetes Allocate Persistent Volumes Explainer

## Source

- Transcript: `raw/transcripts/43 - Where does Kubernetes Allocate Persistent Volumes.md`
- Wiki source page: [[43 - Where does Kubernetes Allocate Persistent Volumes]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 12.00.34 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.03.21 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.04.37 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.04.54 AM.png]]

## What This Lesson Is Answering

At this point in the course, the PVC YAML already exists.

So the next reasonable question is:

when Kubernetes sees that claim, where does it actually get the storage from?

This lesson answers that question by introducing the idea of a `StorageClass`.

## The Local-Machine Story

On a local machine, the answer is usually very simple.

Kubernetes does not have a huge menu of storage systems available.

So when the claim asks for storage, Kubernetes often ends up carving out some space from the host machine's disk.

That is what the first diagram is illustrating:

- the claim asks for storage
- Kubernetes decides how to satisfy it
- locally, the obvious place is the host disk

## How To Inspect The Default

The lesson then shows how to inspect the available storage-class options:

```bash
kubectl get storageclass
kubectl describe storageclass
```

The captured output shows a default storage class:

- `hostpath`

And the provisioner shown is:

- `docker.io/hostpath`

So in this setup, the default behavior is basically:

use host-path-backed storage on the local machine.

## What The Describe Output Adds

The `describe storageclass` output adds a few more useful details:

- it is the default class
- reclaim policy is `Delete`
- binding mode is `Immediate`

You do not need to memorize all of those yet.

The important point for this lesson is simpler:

Kubernetes is not guessing randomly.

It is following the default `StorageClass` unless you tell it otherwise.

## Why Cloud Changes The Story

The lesson then broadens the picture.

On a cloud provider, Kubernetes is no longer limited to "just use the local hard drive."

There may be many different storage backends available.

The course gives examples such as:

- Google Cloud Persistent Disk
- Azure File
- Azure Disk
- AWS Block Store

So the important contrast is:

- local development often has one easy default
- cloud deployments often have many possible storage provisioners

That is why `StorageClass` matters.

## Why This Matters

This lesson makes the storage story much more realistic.

The PVC is not magic.

It is just a request.

Something still has to decide how that request becomes actual storage.

The `StorageClass` is a big part of that answer.

## What Is Still Missing

This lesson still does not show:

- an explicit `storageClassName` in the PVC
- the exact YAML binding to the Postgres pod

So the storage story is now:

- claim exists
- access mode and size are understood
- default allocation behavior is understood

But the final pod wiring still remains.

## Best Reuse

Use this note when you want to remember:

- where local persistent storage usually comes from
- how to inspect the default storage class
- why cloud environments make storage provisioning more flexible and more complex
- why PVCs still depend on a provisioner strategy behind the scenes
