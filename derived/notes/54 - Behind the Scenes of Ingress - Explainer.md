# Behind the Scenes of Ingress Explainer

## Source

- Transcript: `raw/transcripts/54 - Behind the Scenes of Ingress.md`
- Wiki source page: [[54 - Behind the Scenes of Ingress]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 12.39.03 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.40.08 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.41.13 PM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 12.44.15 PM.png]]

## What This Lesson Is Doing

Ingress can feel abstract if you only think of it as “some networking thing.”

This lesson makes it easier by comparing ingress to something you already know from earlier in the course:

`Deployment`

## The Deployment Analogy

The course reminds you that a deployment is a controller.

It watches:

- current state
- desired state

And it works until the actual cluster matches the desired cluster state.

That is the controller idea.

## Applying That Idea To Ingress

Ingress works the same way at a high level.

You write an ingress config that says:

"These are my routing rules."

Then an ingress controller takes responsibility for making those routing rules true inside the cluster.

So ingress is not just “a file.”

It is:

- a config object
- plus a controller that makes the config matter

## The Generic Mental Model

The generic architecture in this lesson has three moving parts:

1. ingress config
2. ingress controller
3. something that actually accepts traffic and routes it

That is the clean conceptual model.

## The Ingress-Nginx Twist

The lesson then says:

for the specific project being used here, `ingress-nginx`, those last two pieces are very tightly combined.

So instead of thinking:

- controller over here
- traffic router over there

you can think:

- ingress config
- ingress-nginx deployment/pod that both watches the config and performs the routing

That is the project-specific simplification.

## Why This Matters

This lesson matters because it gives you the right mental model before installation.

Without it, ingress just looks like magic.

With it, ingress becomes another example of the Kubernetes pattern you have already seen:

- declare desired state
- let a controller reconcile reality to match it

## Best Reuse

Use this note when you want to remember:

- why ingress is not just a one-off network object
- how ingress fits the controller pattern
- what the ingress config is doing
- why ingress-nginx feels simpler than the generic three-part mental model
