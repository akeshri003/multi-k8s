# Rebuilding the Client Image Explainer

## Source

- Transcript: `raw/transcripts/21 - Rebuilding the Client Image.md`
- Wiki source page: [[21 - Rebuilding the Client Image]]

## What This Lesson Adds

The previous lesson prepared the deployment.

This lesson prepares the image itself.

The goal is to create a newer `multi-client` image that is easy to recognize in the browser, so the later deployment update will be obvious.

## Step 1: Change The App Code

The lesson goes back to the old `complex` project and edits the client React app.

The chosen change is deliberately simple:

```text
Fib calculator
```

becomes:

```text
Fib calculator version two
```

That is a good teaching choice because it gives you a clear visual proof later.

If the browser still says the old text, the deployment is still on the old image.

If the browser says `version two`, the new image made it through.

## Step 2: Rebuild The Image

Once the source code changes, the Docker image has to be rebuilt:

```bash
docker build -t <docker-id>/multi-client .
```

This matters because Kubernetes does not run your source files directly. It runs containers built from images.

So changing the React code alone changes nothing in the cluster until a new image exists.

## Step 3: Push The Image

After the build, the new image is pushed:

```bash
docker push <docker-id>/multi-client
```

Now Docker Hub has the newest image contents.

That solves the registry side of the problem.

But it does not solve the Kubernetes side yet.

## Why This Lesson Matters

This lesson separates two ideas that beginners often blur together:

- creating a newer image
- getting Kubernetes to use that newer image

Those are not the same thing.

By the end of this lesson:

- the new image exists
- the new image is in Docker Hub
- the deployment may still be running old pods from the old image

That gap is exactly what the next lesson is about.

## Best Reuse

Use this note when you want a simple reminder of:

- why the app text was changed
- why the Docker rebuild is necessary
- why pushing to Docker Hub still does not automatically refresh deployment pods
