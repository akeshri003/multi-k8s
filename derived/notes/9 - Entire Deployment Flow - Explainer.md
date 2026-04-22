# Entire Deployment Flow Explainer

## Source

- Transcript: `raw/transcripts/9 - Entire Deployment Flow.md`
- Wiki source page: [[9 - Entire Deployment Flow]]

## What This Lesson Is Trying To Teach

Earlier lessons showed the first commands and the first YAML files. This lesson answers the next question:

What is Kubernetes actually doing after we run `kubectl apply`?

The answer is not "it just starts one container."

The lesson's real point is that Kubernetes behaves more like a control system:

- you describe the state you want
- Kubernetes checks the real cluster
- Kubernetes keeps working until the real cluster matches that requested state

That is the key mental model to keep.

## The Opening Demo: Why The Restart Count Matters

The transcript starts with a live demonstration using:

```bash
kubectl get pods
docker ps
docker kill <container-id>
```

The instructor manually kills the container that is running inside the pod. Then the workload appears again, and the pod shows a higher restart count.

That matters because it demonstrates the central promise of this lesson:

Kubernetes is not passively hosting containers. It is actively trying to maintain the state you asked for.

## The Simple Diagram Versus The Realer Diagram

The first screenshot marks the shift from the earlier simple picture to a more detailed one:

![[raw/assets/Screenshot 2026-04-17 at 11.07.03 PM.png]]

The next two visuals carry most of the lesson:

![[raw/assets/Screenshot 2026-04-17 at 11.10.28 PM.png]]

![[raw/assets/Screenshot 2026-04-17 at 11.20.18 PM.png]]

These diagrams show four main pieces:

- a deployment file on the left
- the master at the bottom
- several worker nodes on the right
- Docker Hub at the top as the image source

## Step-By-Step Flow

Here is the flow in plain language.

### 1. The developer writes a file

The transcript says to imagine a deployment file that asks for:

```text
4 copies of multi-worker
```

This is important because the lesson is now moving beyond "run one pod" and into "run several copies of a workload."

### 2. The developer sends the file with kubectl

The file is applied through `kubectl`.

The important rule is:

- you do not manually log into nodes and start containers yourself
- you send instructions to Kubernetes through `kubectl`

The lesson is very explicit about this boundary.

### 3. The file goes to the master

The transcript uses the older word `master` for the control machine.

It also says the real system has several control-plane programs, but simplifies them into one teaching character:

```text
Kube API Server
```

This simplified component reads the configuration and understands what the developer wants the cluster to be doing.

### 4. Kubernetes records the desired state

The lesson describes this as a little internal "list of responsibilities."

That is a very good beginner mental model.

In simple terms, Kubernetes stores something like:

```text
Need: 4 copies of multi-worker
Currently running: 0
```

This gap is the reason the system takes action.

### 5. The master assigns work to nodes

The master looks across the available nodes and says, in effect:

- you run one copy
- you run one copy
- you run two copies

The exact split can vary, but the key idea is that Kubernetes can distribute work across machines.

### 6. Each node pulls the image and starts containers

The transcript emphasizes that each node has Docker running locally.

So each node can:

1. reach out to Docker Hub
2. pull the needed image
3. store it locally
4. start containers from that image

The lesson also makes one important simplification explicit:

- the diagram draws containers directly
- but in reality those containers are running inside pods

That simplification is fine for the lesson as long as you remember the pod boundary is still there.

## The Most Important Idea: Desired State Versus Actual State

This is the heart of the lesson.

Kubernetes is always comparing:

- desired state
- actual state

If desired state says:

```text
4 copies should exist
```

but actual state says:

```text
only 3 copies are running
```

then Kubernetes treats that as a problem to fix.

This is why the earlier `docker kill` demo matters so much.

When one container dies:

1. the system notices the missing copy
2. the master updates its internal count
3. a node is told to create a replacement
4. the cluster returns to the requested number of copies

This is the self-healing story in very simple form.

## What You Should And Should Not Do As A Developer

One of the most useful practical lessons here is about how you interact with the cluster.

The lesson says you should think like this:

- you work with `kubectl`
- `kubectl` talks to the master
- the master decides what nodes should do

You should not think like this:

- pick a worker node manually
- shell into it
- start or restart containers there by hand as your normal workflow

That would bypass the whole orchestration model.

## Why This Lesson Matters

This lesson gives the first real explanation of why Kubernetes is more than Docker plus a few extra commands.

It introduces:

- central coordination
- distribution across nodes
- image pulling on demand
- continuous monitoring
- automatic replacement when something dies

That is the beginning of the real Kubernetes mental model.

## Important Caveats

- The lesson uses older terminology like `master`.
- The control plane is simplified into `Kube API Server` for teaching purposes.
- The lesson talks about a "deployment file," but it does not yet fully unpack the higher-level object model that usually manages replicas in modern Kubernetes.

Those simplifications are fine for now, but they should be remembered as simplifications.

## Best Reuse

Use this note when you want the simple explanation for:

- what happens after `kubectl apply`
- why Kubernetes can recreate crashed workloads
- what "desired state" means
- why developers talk to the control layer instead of directly managing worker nodes

