# Environment Variables as Strings Explainer

## Source

- Transcript: `raw/transcripts/50 - Environment Variables as Strings.md`
- Wiki source page: [[50 - Environment Variables as Strings]]

## Key Visuals

- ![[Screenshot 2026-04-20 at 11.41.03 AM.png]]
- ![[Screenshot 2026-04-20 at 11.41.14 AM.png]]
- ![[Screenshot 2026-04-20 at 11.41.39 AM.png]]

## What This Lesson Fixes

The previous lesson wired the secret correctly, but `kubectl apply` failed with:

```text
cannot convert int64 to string
```

This lesson shows the real cause:

some environment-variable values were written as numbers, but Kubernetes expects those `value` fields to be strings.

## Why The Error Happened

In YAML, these two things are not the same:

```yaml
value: 6379
```

and

```yaml
value: "6379"
```

The first one is a number.

The second one is a string.

For environment variables, Kubernetes wants the string form.

That is why the earlier apply failed.

## The Worker Fix

The worker deployment changes:

```yaml
- name: REDIS_PORT
  value: "6379"
```

This is the only numeric env value shown for the worker.

## The Server Fix

The server deployment changes:

```yaml
- name: REDIS_PORT
  value: "6379"
- name: PGPORT
  value: "5432"
```

These were previously the numeric-looking values causing trouble in the `env` block.

## What The Terminal Output Proves

The last screenshot is important because it shows both sides of the story:

- first `kubectl apply -f k8s` fails with the `int64` error
- after quoting the values, `kubectl apply -f k8s` succeeds

That means the secret wiring from the previous lesson was fine.

The real issue was just YAML typing.

## Why This Lesson Matters

This is a very practical Kubernetes lesson:

sometimes the architecture is correct, the object references are correct, and the secret usage is correct, but the manifest still fails because a field is typed wrong.

So this lesson is really about respecting the schema, not changing the design.

## Source Hygiene Note

The screenshots for this lesson are embedded in the notes and source page, but they currently live at the vault root rather than under `raw/assets`.

So they are usable, but not yet stored in the canonical raw-assets location.

## Best Reuse

Use this note when you want to remember:

- why the earlier `int64` error happened
- why env `value` entries should be quoted when they are numeric-looking
- which fields needed quoting in the worker and server manifests
- how a small YAML type fix can unblock the whole apply step
