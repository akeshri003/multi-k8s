# Persistent Volume Access Modes Explainer

## Source

- Transcript: `raw/transcripts/42 - Persistent Volume Access Modes.md`
- Wiki source page: [[42 - Persistent Volume Access Modes]]

## Key Visual

- ![[raw/assets/Screenshot 2026-04-19 at 11.57.16 PM.png]]

## What This Lesson Is Doing

The previous lesson created the PVC file.

This lesson explains what the important fields inside that file actually mean.

So the goal here is interpretation, not new architecture.

## Start With The Right Mental Model

The lesson repeats an important warning:

a `PersistentVolumeClaim` is not the storage itself.

It is a requirement sheet.

It tells Kubernetes:

"Find me storage that matches these rules."

That is why the fields inside `spec` matter.

## The Three Access Modes

The access mode tells Kubernetes what kind of shared usage pattern the storage must support.

The course introduces three modes:

- `ReadWriteOnce`
- `ReadOnlyMany`
- `ReadWriteMany`

Their meanings are simple:

- `ReadWriteOnce` = one node can read and write
- `ReadOnlyMany` = many nodes can read
- `ReadWriteMany` = many nodes can read and write

For this course's Postgres setup, the claim uses:

```yaml
accessModes:
  - ReadWriteOnce
```

That fits the current architecture because the course is using a single Postgres instance, not a multi-node shared-write database setup.

## What `storage: 2Gi` Means

The second field is:

```yaml
resources:
  requests:
    storage: 2Gi
```

This is the requested storage capacity.

The transcript treats it very simply:

Kubernetes must find or create storage with that amount of space.

The instructor also makes clear that `2Gi` is overkill for the tiny demo database.

So again, the number is not the important part.

The important part is that the claim can specify a capacity requirement at all.

## Why This Matters

This lesson matters because it turns the PVC from "some file you copy" into something readable.

Now you can look at the spec and understand:

- who can use the storage
- how much storage is being requested

That makes the later Postgres wiring much easier to reason about.

## What Is Still Missing

This lesson still does not answer:

- where Kubernetes will create the storage by default
- which storage backends support which access modes
- how the Postgres pod will attach the claim

Those are the next missing pieces in the sequence.

## Best Reuse

Use this note when you want to remember:

- what the three PVC access modes mean
- why the current course example uses `ReadWriteOnce`
- what `storage: 2Gi` is actually expressing
- why the PVC is still a request, not a real disk by itself
