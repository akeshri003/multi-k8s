# Persistent Volume vs Persistent Volume Claim Explainer

## Source

- Transcript: `raw/transcripts/40 - Persistent Volume vs Persistent Volume Claim.md`
- Wiki source page: [[40 - Persistent Volume vs Persistent Volume Claim]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 11.11.45 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.12.46 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.15.30 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.17.17 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.18.56 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 11.19.33 PM.png]]

## Why This Lesson Exists

The previous lesson already explained that `PersistentVolume` is better than a plain pod-level `Volume` for Postgres.

But that still leaves a practical question:

how does a pod actually get one?

This lesson answers that by separating two different things that are easy to confuse:

- the request for storage
- the actual storage resource

## The Hard-Drive Store Analogy

The course uses a simple analogy.

Imagine you are building a custom computer and you still need a hard drive.

You see a billboard that advertises available storage options:

- `500 GB`
- `1 TB`

You walk into the store and ask for one of those options.

From there, two things can happen:

- the store already has the drive in stock
- or the store has to get one created for you

Either way, you still receive storage.

That is the whole setup for the Kubernetes mapping.

## What Maps To What

In the course's mapping:

- the billboard = `PersistentVolumeClaim`
- the actual hard drives = `PersistentVolume`
- the salesperson = Kubernetes

The main idea is:

a `PersistentVolumeClaim` is not the disk itself.

It is the request layer.

It says:

"I need storage with these characteristics."

The `PersistentVolume` is the actual storage that fulfills that request.

## The Simplest Way To Remember It

If the names feel too abstract, reduce them to verbs:

- `PersistentVolumeClaim` = ask
- `PersistentVolume` = hold data

That is the cleanest mental model in this lesson.

The claim asks.

The volume stores.

## Static Versus Dynamic Provisioning

The lesson then adds one more useful distinction.

Sometimes the storage already exists ahead of time.

In that case, Kubernetes can immediately match your claim to an already-created `PersistentVolume`.

The course calls that:

- statically provisioned storage

Other times, the storage does not already exist.

In that case, Kubernetes creates the persistent storage only when the request arrives.

The course calls that:

- dynamically provisioned storage

So there are really two separate distinctions in this lesson:

1. claim versus actual storage
2. pre-created storage versus on-demand storage

## What The Developer Actually Controls

From the developer's point of view, the most important thing is not manually attaching some low-level disk by hand.

The important thing is describing the storage you want.

That is why the lesson focuses on the claim.

You describe the need.

Kubernetes tries to satisfy it by either:

- finding a matching persistent volume
- or creating one for you

That is a much cleaner model than forcing every pod definition to know exactly which physical storage device to mount.

## Why This Matters For Postgres

This is the missing bridge between the earlier persistence lessons and the real Postgres setup.

The course already proved:

- Postgres data must survive pod replacement
- a pod-level `Volume` is not enough

This lesson adds:

- the pod will not talk directly in terms of "give me disk number 7"
- instead, the pod asks through a `PersistentVolumeClaim`
- Kubernetes then matches that claim to a real `PersistentVolume`

That is the storage model the course is building toward for the database.

## What Is Still Missing

Even after this lesson, one important thing is still not shown:

the actual YAML.

So the main value of this lesson is conceptual.

It gives you the right mental model before the configuration appears.

## Best Reuse

Use this note when you want to remember:

- the difference between `PersistentVolumeClaim` and `PersistentVolume`
- why a claim is not the same thing as the storage itself
- how static and dynamic provisioning differ
- why Kubernetes inserts a request layer between the pod and the underlying storage
