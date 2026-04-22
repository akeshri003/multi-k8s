# Imperatively Updating a Deployment's Image Explainer

## Source

- Transcript: `raw/transcripts/23 - Imperatively Updating a Deployment's Image.md`
- Wiki source page: [[23 - Imperatively Updating a Deployment's Image]]

## What This Lesson Finishes

The previous lesson ended with a problem:

- a new image exists in Docker Hub
- the deployment manifest did not change
- `kubectl apply -f` alone is not enough

This lesson finishes that story by showing the full workaround the course prefers in practice.

## Step 1: Build A Versioned Image

Instead of rebuilding the image with the same vague reference, the lesson now adds a real version tag:

```bash
docker build -t <docker-id>/multi-client:v5 .
```

The exact tag value is arbitrary. The point is that it is unique.

That uniqueness matters because it gives Kubernetes a specific image reference to move to.

## Step 2: Push The Tagged Image

After the build, the tagged image is pushed:

```bash
docker push <docker-id>/multi-client:v5
```

At this point Docker Hub has a clear, named image version that Kubernetes can target directly.

## Step 3: Use `kubectl set image`

Now comes the key command:

```bash
kubectl set image deployment/client-deployment client=<docker-id>/multi-client:v5
```

This is the concrete imperative step the last lesson only described conceptually.

The command is long, but the structure is simple once you break it apart:

- `kubectl`
  the CLI talking to the cluster
- `set`
  update a property
- `image`
  the property being changed
- `deployment/client-deployment`
  the object to update
- `client`
  the container name inside that deployment's pod template
- `<docker-id>/multi-client:v5`
  the exact new image reference to use

So the command is effectively saying:

go to this deployment, find this container, and change its image to this versioned tag.

## Step 4: Verify The Pod Was Recreated

After the command, the lesson checks:

```bash
kubectl get pods
```

The important clue is a very small pod age.

That means the old pod was replaced and the deployment created a new one from the updated template.

This is the operational proof that the image update actually triggered a rollout.

## Step 5: Verify In The Browser

The final proof is user-facing.

The lesson opens the app through the normal service path and expects to see:

```text
Fib calculator version two
```

That closes the loop:

- source code changed
- image rebuilt
- image pushed
- deployment updated
- browser shows the new behavior

## Why The Browser Can Still Look Wrong For A Moment

The transcript adds an important practical warning:

React and the browser can cache aggressively.

So even when the deployment update has actually worked, the browser may still show the old page briefly.

That is why the lesson recommends:

- waiting a moment and refreshing
- disabling cache in DevTools
- rerunning `kubectl set image` if needed

That is not a new Kubernetes concept. It is just a reminder that application delivery has more than one caching layer.

## Why This Lesson Matters

This is the point where the course stops talking abstractly about image update strategies and finally gives a usable command.

The important practical model becomes:

1. build a uniquely tagged image
2. push it
3. point the deployment at that new tag

That is the real bridge between "my code changed" and "my cluster is running the new code."

## Important Caveat

The solution works, but it is not purely declarative.

The course openly accepts that tension and says the real answer in production is automation:

- scripts
- CI
- deployment tooling

So the manual command is teaching the mechanism, not necessarily the final human workflow.

## Best Reuse

Use this note when you want a simple explanation of:

- why version tags matter for deployment updates
- what `kubectl set image` actually means
- how to verify that the rollout really happened
- why browser caching can briefly hide a successful update
