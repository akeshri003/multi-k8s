# Postgres Environment Variable Fix Explainer

## Source

- Transcript: `raw/transcripts/49 - Postgres Environment Variable Fix.md`
- Wiki source page: [[49 - Postgres Environment Variable Fix]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 11.37.52 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 11.38.06 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 11.38.34 AM.png]]

## What This Lesson Finishes

The previous lesson created the secret itself.

But a secret sitting in the cluster does nothing until a deployment actually reads from it.

This lesson adds that missing consumption step.

It updates:

- the server deployment
- the Postgres deployment

## The Server Change

The server deployment gets a new env entry:

```yaml
- name: PGPASSWORD
  valueFrom:
    secretKeyRef:
      name: pgpassword
      key: PGPASSWORD
```

This means:

- create an environment variable called `PGPASSWORD`
- do not hardcode its value
- pull its value from the `pgpassword` secret
- specifically from the `PGPASSWORD` key inside that secret

The transcript also makes an important point:

the env var name inside the container and the secret name are not automatically the same thing.

They just happen to look similar here.

What really matters is:

- the container expects a specific env var name
- the secret has a specific object name
- the secret stores one or more key/value entries

Those are three separate naming layers.

## The Postgres Change

The Postgres deployment gets its own env entry:

```yaml
env:
  - name: POSTGRES_PASSWORD
    valueFrom:
      secretKeyRef:
        name: pgpassword
        key: PGPASSWORD
```

This is the other half of the wiring.

Now:

- the server knows which password to use when connecting
- the Postgres container knows which password to require

That is the basic secret-backed password alignment the course needed.

## Why `POSTGRES_PASSWORD` And `PGPASSWORD` Differ

This lesson quietly shows an important practical rule:

different containers may expect different environment variable names even when they are using the same underlying secret value.

So:

- the app server wants `PGPASSWORD`
- the Postgres image wants `POSTGRES_PASSWORD`

Both can still read from the same secret key.

That is one reason `valueFrom.secretKeyRef` is useful.

## The Apply Error

After wiring the secret in, the lesson runs:

```bash
kubectl apply -f k8s
```

And Kubernetes rejects the patch with:

```text
cannot convert int64 to string
```

That is not a secret problem.

It is a YAML typing problem.

The likely issue is that some env values such as:

- `6379`
- `5432`

are still being written as numeric values in places where Kubernetes expects strings for environment variables.

So this lesson ends on a very realistic note:

the secret wiring is conceptually correct, but the YAML still needs one more cleanup pass before the apply works.

## Why This Lesson Matters

This lesson is the bridge between:

- creating a secret
- actually using a secret

That is a major Kubernetes skill boundary.

It also introduces a very common real-world issue:

YAML is not just about shape, it is also about types.

So even when the configuration idea is correct, the manifest can still fail if a field is typed incorrectly.

## Best Reuse

Use this note when you want to remember:

- how `valueFrom.secretKeyRef` looks in a deployment
- how one secret can feed different env var names in different containers
- why the server and Postgres both need the same underlying password
- why the next lesson is going to be about turning numeric env values into strings
