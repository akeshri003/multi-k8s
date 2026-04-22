# Designating a PVC in a POD Template Explainer

## Source

- Transcript: `raw/transcripts/44 - Designating a PVC in a POD Template.md`
- Wiki source page: [[44 - Designating a PVC in a POD Template]]

## What This Lesson Finishes

The earlier lessons already gave you:

- the Postgres deployment
- the PVC manifest
- the meaning of the PVC fields
- the default storage-allocation story

This lesson finally connects those pieces.

It answers:

how does the Postgres pod actually use the claim?

## The Three-Part Wiring

The lesson is easiest to understand as three linked layers.

### 1. The Claim Already Exists

The PVC has already been defined elsewhere:

```yaml
metadata:
  name: database-persistent-volume-claim
```

That claim says what kind of storage Postgres needs.

### 2. The Pod Template Requests That Claim

Inside the Postgres deployment's pod template, the lesson adds:

```yaml
volumes:
  - name: postgres-storage
    persistentVolumeClaim:
      claimName: database-persistent-volume-claim
```

This tells Kubernetes:

"When you create this pod, I need storage that satisfies this claim."

This is the point where the deployment starts depending on the PVC.

### 3. The Container Mounts That Storage

Inside the Postgres container config, the lesson adds:

```yaml
volumeMounts:
  - name: postgres-storage
    mountPath: /var/lib/postgresql/data
    subPath: postgres
```

This is what makes the storage usable inside the actual container.

Without `volumeMounts`, the storage may exist for the pod, but the container does not know where to use it.

## Why The Names Must Match

The lesson emphasizes one very important detail:

the `name` inside `volumeMounts` must match the `name` inside `volumes`.

That shared name is the bridge between:

- the pod-level storage definition
- the container-level mount instruction

So Kubernetes reads that as:

"Take the storage I defined as `postgres-storage` and mount it into this container."

## Why The Mount Path Matters

The mount path is:

```yaml
/var/lib/postgresql/data
```

That path matters because it is the location where Postgres writes its data by default.

So the lesson is not mounting the storage at some random folder.

It is mounting it exactly where the database stores its real data.

That is what makes persistence useful here.

## Why `subPath: postgres` Exists

The lesson adds one more field:

```yaml
subPath: postgres
```

The instructor treats this as a Postgres-specific workaround.

The practical takeaway is simple:

instead of treating the whole mounted storage root directly as the final data directory, Postgres writes into a nested `postgres` folder inside that storage.

For this vault, that is the important operational meaning.

The deeper image-specific reason is intentionally glossed over in the transcript, and that is fine for now.

## The Clean Mental Model

If this configuration feels abstract, reduce it to one sentence:

the claim describes the storage, the pod asks for it, and the container mounts it.

That is the whole story of this lesson.

## What Is Still Missing

This lesson shows the final YAML shape, but it still does not do the runtime proof step.

The next practical questions are:

- does the updated deployment apply cleanly?
- does the pod start correctly?
- does the data actually persist across recreation?

So the config story is now mostly complete, but the verification story is still ahead.

## Best Reuse

Use this note when you want to remember:

- where the PVC gets referenced in a deployment
- how `volumes` and `volumeMounts` work together
- why `mountPath` points at the Postgres data directory
- why `subPath: postgres` appears in this specific example
