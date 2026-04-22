# Claim Config files Explainer

## Source

- Transcript: `raw/transcripts/41 - Claim Config files.md`
- Wiki source page: [[41 - Claim Config files]]

## What This Lesson Is Doing

The previous lesson explained the idea of:

- `PersistentVolume`
- `PersistentVolumeClaim`

This lesson turns that idea into a real config file.

So this is the first point where the storage discussion stops being just analogy and becomes actual Kubernetes YAML.

## The PVC Manifest

The lesson creates this claim:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: database-persistent-volume-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```

This is the first concrete storage object in the sequence that your Postgres deployment will eventually rely on.

## What The File Means

This file is not saying:

"Here is my database pod."

And it is not saying:

"Here is my actual hard drive."

It is saying:

"When some pod asks for this claim, I want storage that matches these requirements."

That is the important shift.

The claim is the request contract.

## Why `accessModes` Is An Array

The lesson points out that `accessModes` has an `s` because it is written as a list.

Even though this example only uses one entry, Kubernetes still expects the list shape:

```yaml
accessModes:
  - ReadWriteOnce
```

That is just the YAML structure Kubernetes expects here.

## Why `2Gi`

The transcript says `2Gi` is far more than this demo app really needs.

So the number is not there because Postgres truly requires two gigabytes for the course demo.

It is there because the instructor wants a simple, visible storage request example.

The more important point is not the exact number.

The more important point is that the claim is allowed to specify capacity requirements at all.

## Why This Matters

This lesson is important because the storage story now has a real object:

- not just a general idea of durability
- not just a diagram
- not just an analogy

It now has a concrete Kubernetes manifest.

That is the first half of the final storage wiring.

The second half is still missing:

the pod has to actually use this claim.

## What Is Still Missing

This lesson does not yet explain:

- what `ReadWriteOnce` means in practice
- where the storage will be created by default
- how Postgres actually mounts and uses this claim

Those all arrive in the next few lessons.

## Best Reuse

Use this note when you want to remember:

- the first actual PVC YAML in the course
- why the claim is a request and not the storage itself
- what the two core claim fields are in this example
- where the storage sequence stops before the pod-template wiring begins
