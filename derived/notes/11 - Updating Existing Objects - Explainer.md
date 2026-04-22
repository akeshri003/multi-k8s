# Updating Existing Objects Explainer

## Source

- Transcript: `raw/transcripts/11 - Updating Existing Objects.md`
- Wiki source page: [[11 - Updating Existing Objects]]

## What This Lesson Is Trying To Teach

The previous lesson said Kubernetes should usually be used declaratively.

This lesson makes that concrete by asking a simple question:

How do you update something that already exists?

The lesson's answer is intentionally repetitive:

you still use the declarative workflow.

You do not switch to a completely different style just because the object already exists.

## The Example Change

The source uses a very specific example:

- there is already a pod running
- that pod currently uses the `multi-client` image
- the new goal is to make it use the `multi-worker` image instead

These visuals frame that comparison:

![[raw/assets/Screenshot 2026-04-19 at 2.46.24 PM.png]]

![[raw/assets/Screenshot 2026-04-19 at 2.46.52 PM.png]]

The transcript also references a third visual that is currently outside canonical `raw/` storage:

![[Screenshot 2026-04-19 at 2.49.24 PM.png]]

## The Imperative Version

The lesson first describes what an imperative update would feel like.

You would:

1. list the current pods
2. inspect the current state
3. identify the specific pod you want to change
4. look up or invent the exact command that updates that pod's image

The transcript's point is not that this is impossible.

The point is that the human still has to do the planning work:

- inspect current state
- figure out the migration strategy
- choose the exact action

That is the same burden the course has been warning about.

## The Declarative Version

The declarative version is much simpler and more repeatable:

1. find the config file that originally created the pod
2. edit that file
3. change the desired field
4. apply the file again through `kubectl`

In this lesson, the desired field is the image.

So the conceptual change is:

```text
from: stephengrider/multi-client
to:   stephengrider/multi-worker
```

To make that easier to picture, here is a normalized illustrative manifest based on the earlier pod config:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: client-pod
  labels:
    component: web
spec:
  containers:
    - name: client
      image: stephengrider/multi-worker
      ports:
        - containerPort: 3000
```

This block is not copied from the transcript directly. It is a readable reconstruction of the exact change the lesson describes.

## How Kubernetes Knows This Is An Update

This is the main conceptual point of the lesson.

The transcript says Kubernetes looks at two fields when you re-apply the config:

- `kind`
- `name`

If the new file says:

```yaml
kind: Pod
metadata:
  name: client-pod
```

and a pod with that same name already exists, the course says Kubernetes will treat the file as an update to that existing object.

If you changed the name, for example to something like:

```yaml
metadata:
  name: client-pod-new
```

then the course says Kubernetes would treat that as a new object and create it instead of updating the old one.

## The Reusable Rule

The lesson is trying to give you a rule you can use again and again:

- do not invent a brand-new update workflow
- go back to the original config
- keep the object's identifying fields stable
- edit the desired state
- apply the file again

That is why the course keeps pushing the declarative model so hard. It gives you one repeated way of working.

## Important Caveat

This lesson teaches a simplified matching rule based on `name` and `kind`, which is fine for the current stage of the course.

But it is still a simplified beginner model. The broader Kubernetes identity story is more precise than this, which is why the vault now also has [[kubernetes-object-identity]] as a separate explanation.

## Best Reuse

Use this note when you want the simple explanation for:

- how to update an existing object declaratively
- why you reuse the original config file
- why keeping `name` and `kind` stable matters
- why declarative updates are easier than imperative one-off commands

