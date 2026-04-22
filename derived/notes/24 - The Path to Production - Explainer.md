# The Path to Production Explainer

## Source

- Transcript: `raw/transcripts/24 - The Path to Production.md`
- Wiki source page: [[24 - The Path to Production]]

## Key Visuals

- ![[raw/assets/Pasted image 20260419201828.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 8.18.14 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 6.24.15 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 6.29.03 PM.png]]

## What This Lesson Changes

Until now, the course has mostly been teaching Kubernetes in small pieces:

- what a pod is
- what a service is
- what a deployment is
- how image updates work

This lesson changes the scale of the conversation.

Instead of learning one Kubernetes object at a time, the course is now asking:

how do we move the full multi-container Fibonacci app into Kubernetes?

That is a much bigger step.

## The New Architecture

The diagram introduces the overall shape of the app inside Kubernetes.

The important parts are:

- `multi-client`
  the browser-facing frontend
- `multi-server`
  the API
- `multi-worker`
  the background worker
- `Redis`
  data store used by the worker flow
- `Postgres`
  persistent database

These pieces are now shown as Kubernetes-managed objects instead of as a mix of containers and outside managed services.

## What Is New In The Diagram

Three terms are especially important because they are new:

### `Ingress Service`

This is introduced as the traffic entry point from outside the cluster.

That is a big change from the earlier `NodePort`-style learning path.

The course is signaling that the simple development access story is giving way to a more production-shaped networking model.

### `ClusterIP`

The diagram attaches `ClusterIP` services to several parts of the system.

The key idea is:

these are internal cluster-facing services, not the earlier developer-facing `NodePort` pattern.

So the app is starting to look more like a real internal service mesh, even though the course has not yet explained all the networking details.

### `Postgres PVC`

This stands for `Persistent Volume Claim`.

The point here is that the database needs storage that survives beyond the life of a single pod.

The lesson does not unpack the mechanics yet, but it clearly signals:

stateful components need a persistence story.

## Why Redis and Postgres Matter Here

The course makes an important comparison to the old Elastic Beanstalk setup.

Earlier, Redis and Postgres were delegated to outside AWS-managed services.

Now, in this Kubernetes path, the course is choosing to run them inside Kubernetes instead.

That is a meaningful architectural shift because it brings more of the system under Kubernetes management.

It also means more config files, more internal networking, and more operational responsibility.

## The Planned Step Sequence

The second diagram gives the execution order:

1. create config files for each service and deployment
2. test the whole system locally on `minikube`
3. create a GitHub/Travis build-and-deploy flow
4. deploy to AWS or Google Cloud

This is useful because it shows that the course is not jumping straight to production.

It is following a staged path:

- define everything locally first
- prove it works locally
- automate builds and deploys
- then move to the cloud

## Why The "Single Node" Framing Matters

The diagram says to imagine everything on one node at first.

That is not claiming the production system must stay that way.

It is just a teaching simplification.

The course explicitly says that once deployed to AWS or Google Cloud, the system can expand to multiple nodes.

So the mental model is:

- start simple locally
- expand later in real production

## Why This Lesson Matters

This lesson is important because it tells you what kind of work is coming next.

You are no longer dealing with one tiny demo manifest.

You are about to create a real set of Kubernetes objects for a multi-service app:

- services
- deployments
- ingress
- persistence

That is why the lesson warns that there will be around ten or twelve config files.

The complexity is increasing, but it is increasing in a purposeful way.

## Important Caveats

- The lesson introduces `Ingress`, `ClusterIP`, and `PVC`, but does not explain them yet.
- The architecture is still high-level; it is not yet a line-by-line manifest walkthrough.
- The course chooses to run Redis and Postgres inside Kubernetes here, but it does not yet discuss when managed cloud services might still be the better choice.

## Best Reuse

Use this note when you want a simple explanation of:

- how the course is moving from toy examples to the full app
- what the main components of the Kubernetes architecture are
- which new concepts are being introduced next
- what the production path looks like from local setup to cloud deployment
