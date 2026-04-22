# Scaling and Chaning Deployments Explainer

## Source

- Transcript: `raw/transcripts/19 - Scaling and Chaning Deployments.md`
- Wiki source page: [[19 - Scaling and Chaning Deployments]]

## What This Lesson Adds

The earlier lessons established three ideas:

- deployments manage pods through a template
- services give stable access even when pods change
- `kubectl get deployments` shows rollout status

This lesson moves from setup into behavior.

It asks:

what actually happens when you keep editing a deployment?

The answer is:

deployments do not just sit there. They actively replace, add, and clean up pods so the real cluster matches the newest template and replica count.

## Part 1: Change The Pod Template

The lesson first changes the deployment's container port away from `3000`.

Then it re-applies the manifest:

```bash
kubectl apply -f client-deployment.yaml
kubectl get deployments
kubectl get pods
kubectl describe pods
```

The lesson emphasizes that `kubectl apply` reports the deployment as `configured`.

That matters because the deployment object itself is still the same deployment. What changed is its desired specification.

The practical evidence comes from `kubectl get pods`.

The pod age becomes very small, around `26 seconds`, which the lesson reads as proof that Kubernetes did not keep the old pod around unchanged. Instead, it created a new pod from the updated template.

That is the main pattern to remember:

when the deployment template changes, Kubernetes can replace managed pods so they match the new desired state.

## Part 2: Verify The New Pod

The lesson then runs:

```bash
kubectl describe pods
```

Because there is only one pod at that moment, it describes all pods and checks the container details.

The transcript says this confirms that the new pod is now using port `999`.

One small caveat is worth preserving: the raw transcript says `99999` once and later reasons about `999`, so the exact port value is inconsistent. The durable point is still clear:

the recreated pod reflects the newest template, not the old one.

## Part 3: Scale The Deployment

Next, the lesson changes:

```yaml
replicas: 1
```

to:

```yaml
replicas: 5
```

Then it applies the manifest again.

This time, the important result is simple:

the deployment creates five pods from the same template.

So a deployment is not only about updating one pod. It is also the object that controls how many identical copies should exist.

That is why deployments are much closer to real application operation than bare pods.

## Part 4: Change The Image And Watch A Rollout

The final change is the most interesting one.

The transcript changes the image from:

```text
multi-client
```

to:

```text
multi-worker
```

Then it quickly runs:

```bash
kubectl get deployments
```

and catches the deployment in the middle of an update.

The observed numbers are:

- `desired = 5`
- `current = 7`
- `up-to-date = 4`
- `available = 4`

This is the first really useful rollout snapshot in the course.

It shows that Kubernetes does not always delete all old pods first and then create all new ones later.

Instead, for a short time:

- some old pods can still exist
- some new pods can already be running
- the total number of pods can be larger than the final desired count

The transcript interprets this as:

- 4 pods already match the newest template
- 3 older pods are still around
- cleanup will finish soon after

Then a later `kubectl get deployments` should settle back to five healthy pods.

## Why This Lesson Matters

This lesson makes deployments feel real.

Before this, a deployment could seem like just a nicer YAML wrapper around a pod.

After this lesson, the practical difference is much clearer:

- deployments recreate pods when the template changes
- deployments scale pod count through `replicas`
- deployments manage rolling replacement rather than forcing you to replace everything manually

That is a much more realistic model for how Kubernetes keeps applications running while changes are being applied.

## One Useful Mental Model

Think of the deployment as the manager and the pods as replaceable workers.

You do not edit each worker by hand.

You change the manager's instructions:

- how many workers should exist
- which image they should run
- what pod template they should follow

Then Kubernetes does the cleanup and replacement work needed to reach that state.

## Important Caveats

- The transcript's exact replacement port value is inconsistent: it says `99999` once and then keeps talking about `999`.
- The lesson explains that browser access will stop after the port change, but the source base still treats local networking details as somewhat simplified and runtime-specific.
- The transcript shows that `current` can exceed `desired`, but it does not yet explain the rollout strategy settings that make that possible.

## Best Reuse

Use this note when you want a simple explanation of:

- how deployment template updates differ from direct pod updates
- why `replicas` is the scaling knob
- how to read a deployment while a rollout is in progress
- why old and new pods can briefly exist together
