# Walkthrough Deployment Config Explainer

## Source

- Transcript: `raw/transcripts/16 - Walkthrough Deployment Config.md`
- Wiki source page: [[16 - Walkthrough Deployment Config]]

## What This Lesson Is Trying To Clarify

The previous lesson gave the first real deployment YAML file.

This lesson answers the next natural question:

What are all these sections actually doing?

It especially focuses on three things:

- `template`
- `replicas`
- `selector.matchLabels`

## The Familiar Top Of The File

The lesson begins by saying the top of the manifest should already feel familiar:

![[raw/assets/Screenshot 2026-04-19 at 3.38.36 PM.png]]

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: client-deployment
```

That means:

- use the `apps/v1` API group/version
- create a `Deployment`
- name it `client-deployment`

So far, this still looks like the same Kubernetes pattern you have already seen with pods and services.

## The Most Important Part: `template`

The lesson then jumps to the most important section:

```yaml
template:
  metadata:
    labels:
      component: web
  spec:
    containers:
      - name: client
        image: stephengrider/multi-client
        ports:
          - containerPort: 3000
```

The main teaching point is:

this whole block is the configuration for every pod the deployment creates.

That is why it looks familiar. It is basically pod configuration nested inside a deployment.

The next visuals reinforce that idea:

![[raw/assets/Screenshot 2026-04-19 at 3.41.19 PM.png]]

![[raw/assets/Screenshot 2026-04-19 at 3.41.34 PM.png]]

So the right way to read it is:

- the deployment is the higher-level manager
- the template is the recipe for each managed pod

## `replicas`

The lesson explains:

```yaml
replicas: 1
```

as the number of identical pods the deployment should maintain.

If you changed it to `5`, the deployment would try to keep five matching pods running, all created from the same template.

That is one of the biggest differences from a direct bare pod manifest.

A pod file creates one pod.

A deployment file declares a desired count of matching pods.

## `selector.matchLabels`

This is the piece that usually feels most confusing at first:

```yaml
selector:
  matchLabels:
    component: web
```

The lesson explains it in simple terms:

- the deployment asks Kubernetes to create pods
- after those pods exist, the deployment still needs a way to find them again
- the selector is how it gets a handle on the pods it manages

So the deployment looks for pods with the label:

```yaml
component: web
```

That is why the template also includes:

```yaml
metadata:
  labels:
    component: web
```

Without that matching label, the deployment would not be able to identify the pods that belong to it.

## Why The Label Seems Duplicated

This lesson directly addresses the obvious annoyance:

why do we write the same label twice?

The answer is:

because the two pieces are doing related but different jobs.

- the template gives the created pods the label
- the selector tells the deployment which label to look for

So even though the text looks duplicated, the role is not redundant.

## The Best Mental Model

The cleanest mental model from this lesson is:

1. the deployment wants some number of pods
2. the template describes what those pods should look like
3. the labels are stamped onto those pods
4. the selector finds those labeled pods so the deployment can manage them

That is the main conceptual bridge from “I have a deployment YAML file” to “I understand how the deployment is tied to the pods it owns.”

## Best Reuse

Use this note when you want a simple explanation of:

- why the pod-like config sits inside `template`
- what `replicas` actually controls
- why `selector.matchLabels` exists
- why the same label appears in both `selector` and `template.metadata.labels`

