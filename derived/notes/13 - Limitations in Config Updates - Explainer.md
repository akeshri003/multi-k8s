# Limitations in Config Updates Explainer

## Source

- Transcript: `raw/transcripts/13 - Limitations in Config Updates.md`
- Wiki source page: [[13 - Limitations in Config Updates]]

## What This Lesson Changes In Your Mental Model

The previous lessons may leave you with a simple rule:

- edit the config file
- apply it again
- Kubernetes updates the object

This lesson adds an important correction:

that workflow is real, but it does not mean every field on a pod can be changed in place.

That is the whole point of the error shown here.

## The Specific Change

The lesson intentionally makes a very small-looking edit:

```text
containerPort: 3000  ->  containerPort: 9999
```

The visual for that setup is:

![[raw/assets/Screenshot 2026-04-19 at 3.17.34 PM.png]]

Then it tries the normal declarative update step:

```bash
kubectl apply -f client-pod.yaml
```

## The Error

Instead of seeing `configured`, Kubernetes rejects the update.

The most important line is:

```text
The Pod "client-pod" is invalid: spec: Forbidden: pod updates may not change fields other than ...
```

The transcript preserves the list of exceptions:

- `spec.containers[*].image`
- `spec.initContainers[*].image`
- `spec.activeDeadlineSeconds`
- `spec.tolerations` with additions only
- a limited case for `spec.terminationGracePeriodSeconds`

So the lesson is showing a real Kubernetes rule:

pod specs are only partly mutable after creation.

## Why This Matters

This is important because the previous lesson succeeded when changing:

```text
image: stephengrider/multi-client
```

to:

```text
image: stephengrider/multi-worker
```

So you might naturally think:

"Great, I can just keep editing any pod field and re-applying the file."

This lesson shows that this is false.

Changing the image worked because image is one of the allowed mutable fields.

Changing `containerPort` fails because Kubernetes does not allow that field to be changed on the existing pod.

## The Most Useful Piece Of Evidence

The error output even shows the rejected diff:

```diff
- ContainerPort: 3000
+ ContainerPort: 9999
```

That makes the lesson very concrete. Kubernetes is not confused about which field changed. It knows exactly what changed and is explicitly refusing that change.

## The Practical Takeaway

The right takeaway is not:

"Declarative updates do not work."

The right takeaway is:

"Declarative updates work, but object-specific mutability rules still apply."

In other words:

- declarative is still the workflow
- but the object type decides what can be changed in place

This is a much better mental model than the oversimplified version from the earlier lesson.

## Why This Pushes You Toward Higher-Level Objects

Even though the transcript does not fully explain the workaround yet, it strongly hints at the next architectural lesson.

If direct pod updates are this limited, then direct pod editing is probably not the long-term production model for evolving applications.

That is why this lesson increases the importance of the vault's open question about which higher-level Kubernetes object should manage ongoing changes more realistically.

## Best Reuse

Use this note when you want a simple explanation of:

- why one declarative pod update succeeded and another failed
- which pod fields are allowed to change in place
- why pod updates are more limited than the earlier beginner model suggested
- why this limitation points toward a higher-level production object model

