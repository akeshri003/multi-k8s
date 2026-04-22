# Applying a PVC Explainer

## Source

- Transcript: `raw/transcripts/45 - Applying a PVC.md`
- Wiki source page: [[45 - Applying a PVC]]

## What This Lesson Does

The previous lessons finished the storage configuration:

- PVC manifest exists
- access modes are understood
- storage-class defaults are explained
- the Postgres deployment now references and mounts the claim

This lesson is the first runtime validation step.

It answers:

did Kubernetes actually create and bind the storage objects the config described?

## The Verification Workflow

The lesson runs:

```bash
kubectl apply -f k8s
kubectl get pods
kubectl get pv
kubectl get pvc
```

That sequence is doing four different checks.

## 1. Re-Apply The Full Config

```bash
kubectl apply -f k8s
```

This updates the cluster with:

- the new claim
- the updated Postgres deployment

The transcript says this should create the claim and reconfigure Postgres.

## 2. Check That Postgres Is Running

```bash
kubectl get pods
```

The lesson expects to see Postgres restarted and running.

That does not prove persistence yet, but it does prove the deployment still comes up after the PVC wiring was added.

## 3. Check The Actual Persistent Volume

```bash
kubectl get pv
```

This is the first time the lesson uses the cluster view of actual persistent volume objects.

That matters because `pv` is where you see the storage instance itself, not just the request.

The transcript specifically points out:

- the persistent volume has a generated name
- it has the expected size
- it shows the access mode
- it shows the claim it is bound to
- its status is `Bound`

That `Bound` status is the key signal that the request and the real storage have been matched together successfully.

## 4. Check The Claim

```bash
kubectl get pvc
```

This shows the claim object itself.

The lesson uses this to reinforce the mental model:

- `pvc` = the request
- `pv` = the actual storage object that satisfied that request

That is why this lesson matters.

It turns the earlier conceptual distinction into something you can actually inspect in the cluster.

## What This Still Does Not Prove

This lesson is careful about one limitation.

It does not yet prove that Postgres data survives a real recreate cycle.

Why not?

Because the app is not yet fully wired to save meaningful data into Postgres.

The transcript says the next missing step is adding environment variables so the server and worker can connect to Redis and Postgres properly.

So the current proof is:

- storage objects exist
- the claim is bound
- Postgres still runs

But the deeper proof:

- write data
- recreate pod
- confirm data survived

is still ahead.

## The Clean Mental Model

This lesson is best remembered as the point where the storage setup becomes visible in the cluster.

Before this lesson, PV and PVC were config ideas.

After this lesson, you can inspect them as real Kubernetes objects.

## Best Reuse

Use this note when you want to remember:

- the first practical command flow for validating a PVC setup
- the difference between `kubectl get pv` and `kubectl get pvc`
- why `Bound` is the key status to look for
- why this still stops short of proving real database persistence
