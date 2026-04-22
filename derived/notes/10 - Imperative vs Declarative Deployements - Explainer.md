# Imperative vs Declarative Deployements Explainer

## Source

- Transcript: `raw/transcripts/10 - Imperative vs Declarative Deployements.md`
- Wiki source page: [[10 - Imperative vs Declarative Deployements]]

## What This Lesson Is Trying To Clarify

The previous lesson explained that Kubernetes keeps working to match actual state to desired state.

This lesson asks the next question:

What is the developer supposed to give Kubernetes?

The answer is the main point of the lesson:

You should usually give Kubernetes a desired state, not a long list of step-by-step repair commands.

That is the difference between declarative and imperative deployment.

## The Quick Review At The Start

The lesson begins by repeating a few core ideas from the earlier architecture discussion:

![[raw/assets/Screenshot 2026-04-17 at 11.26.05 PM.png]]

- Kubernetes is for deploying containerized applications
- nodes are the machines or VMs that run workloads
- the master manages what happens across nodes
- Docker Hub is where the deployable image comes from
- the master chooses placement by default unless you add extra rules
- the master keeps working to satisfy desired state

That review matters because the imperative-versus-declarative comparison only makes sense once desired state is already clear.

## Imperative Deployment

The lesson defines imperative deployment as giving very specific commands.

The idea is:

- create this thing
- delete that thing
- update this one
- restart that one

In other words, the human operator is doing the planning.

The source introduces that contrast with this visual:

![[raw/assets/Screenshot 2026-04-17 at 11.30.52 PM.png]]

## Why Imperative Feels Hard

The transcript gives two examples.

### Example 1: Four Containers Down To Three

These diagrams walk through the first case:

![[raw/assets/Screenshot 2026-04-17 at 11.32.10 PM.png]]

![[raw/assets/Pasted image 20260417233416.png]]

Suppose:

```text
desired state = 3 containers
current state = 4 containers
```

At first glance, that seems easy. Just remove one.

But the instructor's real point is that even this simple case still forces the human to do several jobs:

1. know the desired state
2. inspect the current state
3. compare the two
4. invent the migration plan
5. run the command to make it happen

That is what imperative management means. The operator is calculating the path from the current state to the target state.

### Example 2: Upgrade Only The Right Workloads

The second example is a little more realistic:

![[raw/assets/Screenshot 2026-04-17 at 11.35.30 PM.png]]

Now the problem is not just "remove one container."

It is:

- find all running workloads that use the old `multi-worker` image
- ignore unrelated containers
- update only the correct targets
- work out the right sequence of commands

That is much more error-prone, especially as systems get larger.

## Declarative Deployment

The declarative alternative is presented with these diagrams:

![[raw/assets/Screenshot 2026-04-17 at 11.37.45 PM.png]]

![[raw/assets/Screenshot 2026-04-17 at 11.40.11 PM.png]]

The workflow is much simpler:

1. open the config file
2. update the desired state there
3. send the config back to Kubernetes
4. let the master figure out the migration

The concrete example is updating the image version in the configuration to:

```text
multi-worker:1.23
```

The lesson says Kubernetes then takes over:

- it updates its internal responsibility list
- it finds pods that are not on the desired version
- it removes outdated pods
- it creates new pods with the correct version

The important point is that the human does not plan each individual step.

## The Real Difference

This is the cleanest way to remember the lesson:

- imperative asks the human to compute the migration path
- declarative asks the human to describe the end state

Kubernetes is built to be strongest in the declarative model because it already has the machinery to keep reconciling actual state with desired state.

## Why The Course Cares So Much

The transcript becomes very explicit here:

- Kubernetes supports both styles through `kubectl`
- blog posts often show imperative commands
- the course wants you to prefer declarative workflows
- production deployments should usually stay declarative

That is an important reading habit for later:

if you see advice that says "just run a command that updates this pod in place," the course wants you to be cautious about that style of guidance.

## The Repeated Pattern To Remember

The source says the same cycle will repeat throughout the course:

1. update the configuration file
2. express the new desired state
3. send the file to Kubernetes
4. let Kubernetes do the reconciliation work

That is the durable workflow this lesson is trying to burn in.

## Best Reuse

Use this note when you want a simple explanation of:

- what imperative deployment means
- what declarative deployment means
- why declarative is easier and safer at scale
- why Kubernetes is usually taught and used as a declarative system

