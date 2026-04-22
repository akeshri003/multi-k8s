# Running Containers with Deployments Explainer

## Source

- Transcript: `raw/transcripts/14 - Running Containers with Deployments.md`
- Wiki source page: [[14 - Running Containers with Deployments]]

## What This Lesson Is Trying To Solve

The previous lesson created a problem:

- changing the pod image worked
- changing `containerPort` failed

So now there is an obvious question:

If direct pod updates are this limited, how are we supposed to keep changing our application over time?

This lesson's answer is:

stop treating direct pods as the main object for serious application work, and start using deployments.

## The Shift In Thinking

The first visual frames the problem:

![[raw/assets/Screenshot 2026-04-19 at 3.21.21 PM.png]]

The lesson says the direct pod-update limits seem to break the earlier promise of declarative change.

The fix is not to abandon declarative workflows.

The fix is to use a better object for the job.

## What A Deployment Is

The transcript introduces `Deployment` as a new Kubernetes object.

Its job is to maintain a set of identical pods.

That can mean:

- one pod
- two pods
- three pods
- or more

The important point is not the number. The important point is that the deployment is now the manager.

It watches the pods, checks whether they are healthy, and tries to keep them in the expected state.

## The Pod Versus Deployment Comparison

The lesson draws a strong distinction between the two.

The next visual starts that comparison:

![[raw/assets/Screenshot 2026-04-19 at 3.23.11 PM.png]]

### Pods

The transcript says pods are for:

- one tightly related set of containers
- very simple development use
- learning the basic Kubernetes model

It also says that direct pods are not what we normally use for serious production work.

### Deployments

Deployments are presented as:

- the standard way to run containers for real work
- the normal path in development
- the primary path in production

That is the major transition this lesson is trying to make.

You are supposed to stop thinking:

"My app runs in a pod."

and start thinking:

"My app runs through a deployment, and the deployment manages pods."

## The Pod Template Idea

These diagrams carry the most important new concept:

![[raw/assets/Screenshot 2026-04-19 at 3.23.26 PM.png]]

![[raw/assets/Screenshot 2026-04-19 at 3.26.27 PM.png]]

A deployment contains a pod template.

That template is basically a recipe for the pods it should create.

The lesson's example says the template might define things like:

- container name
- image
- port

So conceptually the template says:

```text
every pod I manage should look like this
```

The deployment then uses that template to create pods that match it.

## Why This Solves The Update Problem

This is the most important reasoning step in the lesson.

If you change the pod template, the deployment can respond by making sure its managed pods match the new template.

The transcript describes that response in simple terms:

- it might update the existing pod
- or it might kill the old pod and replace it with a fresh one that matches the new template

The exact low-level mechanism is not fully unpacked yet, but the practical message is clear:

the deployment gives Kubernetes a better place to manage change than a one-off direct pod object.

That is why the earlier `containerPort` problem pushes us toward deployments.

## The New Durable Mental Model

After this lesson, the best simple model is:

1. pods still matter
2. deployments are now the main interface for running application containers
3. deployments manage pods by using a shared pod template
4. changing the template lets Kubernetes bring the managed pods into line with the new desired state

## Important Caveat

The transcript speaks very confidently here and says deployments lift the practical limitations we just saw with direct pods.

That is the key teaching direction, but the exact deployment manifest and reconciliation details have not been shown yet. So this lesson is still a conceptual bridge rather than the final detailed explanation.

## Best Reuse

Use this note when you want a simple explanation of:

- why direct pod editing is not the long-term workflow
- what a deployment is
- what a pod template is
- why deployments become the main way to run containers in real Kubernetes usage

