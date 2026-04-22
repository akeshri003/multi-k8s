# The ClusterIP Config Explainer

## Source

- Transcript: `raw/transcripts/28 - The ClusterIP Config.md`
- Wiki source page: [[28 - The ClusterIP Config]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 8.21.07 PM.png]]
- ![[raw/assets/Screenshot 2026-04-17 at 10.12.39 PM.png]]

## What This Lesson Is Doing

The previous lesson explained the idea:

- `NodePort` is outside-facing
- `ClusterIP` is inside-facing

This lesson makes that idea concrete by writing the actual frontend `ClusterIP` service.

So this is the first moment where the architecture shift becomes a real config file instead of just a diagram.

## The Config File

The lesson writes this manifest:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: client-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: web
  ports:
    - port: 3000
      targetPort: 3000
```

This is very close to the earlier `NodePort` service manifest, which is exactly the point the instructor is making.

The structure is mostly the same.

The big change is the exposure model.

## Why The Selector Is Still `component: web`

The service still needs to know which pods it should send traffic to.

That is why the selector is:

```yaml
selector:
  component: web
```

This matches the label on the pods created by the `client-deployment`.

So the service does not point to a pod by name.

It points to a group of pods by label.

That is the stable Kubernetes pattern:

- deployments create pods
- pods carry labels
- services use selectors to find the right pods

## Why There Is No `nodePort`

This is the most important difference from the earlier service config.

In the `NodePort` version, the service existed to let traffic come in from outside the cluster.

That is why it needed a `nodePort`.

Here, the goal is different.

This service exists only so that traffic already inside the cluster can reach the frontend pods.

So the config only keeps:

- `port`
- `targetPort`

and removes:

- `nodePort`

That is the cleanest way to remember the lesson:

`ClusterIP` is the same basic service idea, but without outside-world exposure.

## What `port` And `targetPort` Mean Here

The lesson revisits the port naming from the older `NodePort` diagram, but now in the simpler `ClusterIP` case.

The meaning is:

- `port`
  the port other objects inside the cluster use when they talk to the service
- `targetPort`
  the port on the target pod/container that should receive the traffic

In this example, both are `3000`.

So the service says:

if something inside the cluster talks to this service on `3000`, forward that traffic to port `3000` on the selected frontend pods.

## Why The Ports Could Differ But Do Not Here

The instructor briefly says you could map one service port to a different container port.

For example, the service could listen on one port and then forward traffic to a different one on the pod.

But for this frontend example, that would add complexity without helping anything.

So the lesson keeps both values at `3000`.

That keeps the mental model simple:

- service port `3000`
- frontend container port `3000`

## Why This Lesson Matters

This lesson is small, but it is important.

It is the first concrete config that matches the production-shaped architecture introduced earlier.

It shows that the frontend is no longer being exposed the old demo way through a `NodePort`.

Instead, the frontend now has an internal service that other cluster components can reach, and later `Ingress` will become the public entry point.

So this lesson is really about moving from:

outside user -> `NodePort` -> pods

to:

outside user -> `Ingress` -> `ClusterIP` service -> pods

## Important Caveats

- The transcript appears to mis-hear the filename near the start; `client-cluster-ip-service.yaml` is the intended name.
- The lesson does not yet show the `Ingress` config that will use this service.
- The lesson explains port mapping clearly, but it still does not show service DNS names or how another pod would call this service in practice.

## Best Reuse

Use this note when you want to remember:

- what the first real frontend `ClusterIP` manifest looks like
- how it differs from the older `NodePort` service
- why there is no `nodePort` field
- what `port` and `targetPort` mean in an internal-only service
