# Updating Deployment Images Explainer

## Source

- Transcript: `raw/transcripts/20 - Updating Deployment Images.md`
- Wiki source page: [[20 - Updating Deployment Images]]

## What This Lesson Is Doing

The previous lessons showed that deployments can:

- recreate pods
- scale pod counts
- roll from one image to another

This lesson asks a different question:

what if the image name stays the same, but the image contents change?

That is a very normal real-world situation.

You might rebuild `multi-client`, push it to Docker Hub again, and expect Kubernetes to somehow move your pods onto that newer image.

This lesson sets up that exact problem.

## Step 1: Put The Deployment Back Into A Simple State

The deployment is changed in three ways:

```yaml
spec:
  replicas: 1
  template:
    spec:
      containers:
        - image: stephengrider/multi-client
          ports:
            - containerPort: 3000
```

The point is not that these values are magical.

The point is that they make the next experiment easy to observe:

- `multi-client` gives a visible browser page
- `1` replica makes pod replacement easier to track
- port `3000` matches the browser-testable setup from earlier lessons

## Step 2: Re-Apply And Verify

The manifest is applied again:

```bash
kubectl apply -f client-deployment.yaml
```

Then the lesson checks the app in the browser by using:

```bash
minikube ip
```

and visiting:

```text
<node-ip>:31515
```

That confirms the deployment is back in a clean, testable state.

## Step 3: Frame The Real Problem

Now the lesson introduces the real sequence:

1. rebuild the `multi-client` image
2. push it to Docker Hub
3. get the deployment to recreate pods using that newest image

The important point is that step 3 is not automatically solved just because the registry now has fresher image contents.

That gap is the real topic of the next two lessons.

## Why This Lesson Matters

Without this reset, the image-update discussion would be harder to verify.

The deployment was previously using `multi-worker`, multiple replicas, and a changed port. That made the environment noisier.

This lesson strips the setup back down so you can clearly see when a browser-visible image update actually happens.

## Best Reuse

Use this note when you want to remember:

- why the course temporarily returns to `multi-client`
- why replicas go back to `1`
- why port `3000` comes back
- how the image-update problem is being staged before the actual solutions are discussed
