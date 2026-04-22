# Applying a Deployment Explainer

## Source

- Transcript: `raw/transcripts/17 - Applying a  Deployment.md`
- Wiki source page: [[17 - Applying a Deployment]]

## What This Lesson Is Trying To Do

The previous lessons built up to the first deployment manifest.

This lesson finally runs it.

But before doing that, it cleans up the old world:

- there is still a pod from the earlier direct `client-pod.yaml`
- that old pod would make the cluster state harder to read

So the lesson first removes the old pod, then applies the deployment, then inspects the new state.

## Step 1: Delete The Old Pod

The first commands are:

```bash
kubectl get pods
kubectl delete -f client-pod.yaml
```

The recorded result is:

```text
pod "client-pod" deleted from default namespace
```

The lesson explains that this works because Kubernetes reads the manifest, looks at its `kind` and `name`, finds the matching object, and deletes it.

This is important because it is one of the first places where the course openly says:

this is an imperative command.

That is a useful nuance. The course strongly prefers declarative workflows for creating and changing desired state, but deletion is presented here as a place where imperative control still shows up.

## Why The Delete Takes Time

The transcript also explains why deletion is not always instant.

The pod gets time to shut itself down first, much like stopping a container in Docker. After that grace period, Kubernetes removes it fully.

That is why `kubectl delete` may appear to hang briefly before returning.

## Step 2: Apply The Deployment

Once the old pod is gone, the lesson runs:

```bash
kubectl apply -f client-deployment.yaml
```

This creates the deployment object, and the deployment then creates the pod it wants.

The important mental shift is:

you are no longer directly creating the pod yourself.

You are creating the deployment, and the deployment is responsible for the pod.

## Step 3: Inspect The New Pod

The lesson then checks:

```bash
kubectl get pods
```

The key thing it wants you to notice is that the pod name is no longer the fixed old name `client-pod`.

Instead, it now has a generated name tied to the deployment.

That matters because it shows that the pod is now a managed child of the deployment rather than a one-off directly created object.

## Step 4: Inspect The Deployment Itself

The biggest new command in this lesson is:

```bash
kubectl get deployments
```

This is the deployment-level version of the earlier `get` workflow.

The transcript explains the most important columns:

### `desired`

How many pods the deployment wants, based on `replicas`.

### `current`

How many pods currently exist.

### `up-to-date`

How many pods are using the latest deployment template.

The lesson hints that this becomes especially important after future template changes.

### `available`

How many pods are actually running successfully and able to serve traffic.

## Why This Lesson Matters

This is the point where the course stops talking about deployments as an abstract better idea and starts treating them as the actual operating interface.

The durable workflow now looks more like:

1. clean up outdated direct objects when needed
2. apply the deployment manifest
3. inspect pods
4. inspect deployments

That is a much more realistic Kubernetes loop than the earlier bare-pod flow.

## Important Caveat

The lesson still has not shown the final browser-access step with this new deployment-managed pod. So the container-management side is now clearer than the service-wiring side.

It also introduces imperative deletion as a practical necessity, which is useful to remember because it prevents the declarative story from becoming too absolute.

## Best Reuse

Use this note when you want a simple explanation of:

- why the old pod is deleted before the deployment is applied
- what `kubectl delete -f` is doing
- why the new pod has a generated name
- what `desired`, `current`, `up-to-date`, and `available` mean in `kubectl get deployments`

