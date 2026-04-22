# Connecting to Running Containers Explainer

## Source

- Transcript: `raw/transcripts/8 - Connecting to Running Containers.md`
- Wiki source page: [[8 - Connecting to Running Containers]]

## What This Lesson Adds

The earlier notes introduced the pod manifest and the service manifest separately. This lesson shows how they become a working system.

The new value here is operational:

- how to load both manifests into the cluster
- how to verify that the pod and service actually exist
- how to reach the running application from the browser
- why the address depends on the local Kubernetes runtime

## Step 1: Apply The Manifests

The first move is to load both YAML files into the cluster:

![[raw/assets/Screenshot 2026-04-17 at 10.48.55 PM.png]]

```bash
kubectl apply -f client-pod.yaml
kubectl apply -f client-node-port.yaml
```

The note also preserves the captured output:

```text
aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl apply -f client-pod.yaml

pod/client-pod created

aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl apply -f client-node-port.yaml

service/client-node-port created
```

The important idea is that `kubectl apply` changes cluster state. It is not just reading YAML. It is asking Kubernetes to create or update real objects from that configuration.

## Step 2: Verify With kubectl get

The transcript is careful about not trusting the fast success message too much. That is a useful habit.

The real verification step is:

```bash
kubectl get pods
kubectl get services
```

The pod view tells you whether the pod is actually running, how many replicas are ready, whether it has restarted, and how old it is.

![[raw/assets/Screenshot 2026-04-17 at 10.53.07 PM.png]]

The service view tells you whether the `NodePort` service exists and which externally exposed port it is using.

![[raw/assets/Screenshot 2026-04-17 at 10.55.35 PM.png]]

This is the practical distinction:

- `kubectl apply`
  requested the change
- `kubectl get`
  checks whether the system now matches that request

That is a useful mental model for all later Kubernetes work.

## Step 3: Use The Right Address

This is the part that often feels unintuitive at first.

The source explains that with a `minikube` setup, the Kubernetes node is running inside a VM, so the `nodePort` is exposed on that VM, not directly on your machine's `localhost`.

The recorded reminder command is:

```bash
minikube ip
```

And the conceptual diagram is:

![[raw/assets/Screenshot 2026-04-17 at 10.58.19 PM.png]]

The model is:

1. your browser sends traffic to the node IP plus `nodePort`
2. the node receives the request
3. the service forwards traffic to the target pod
4. the pod serves the application

So in a `minikube` flow, the address is roughly:

```text
<minikube-ip>:31515
```

not:

```text
localhost:31515
```

## How This Fits With The Study Notes Above The Transcript

The top of the source file adds an important local caveat:

- `3050` is the service port other pods would use
- `31515` is the developer-facing `nodePort`
- Docker Desktop Kubernetes may allow `localhost`, unlike the `minikube` VM setup described in the transcript

That means the safest reusable rule is not "never use localhost." The safer rule is:

- if the cluster runs inside a separate VM like `minikube`, use the VM IP
- if the runtime integrates networking more directly, like Docker Desktop often does, `localhost` may work

## Why This Lesson Matters

This is the first point where Kubernetes stops being only a set of definitions and starts feeling like an actual operator workflow:

- write manifests
- apply manifests
- inspect cluster state
- connect through the service boundary

It also reinforces a key networking rule from the earlier service lesson: you do not browse directly to the pod. You browse through the service, using the service's externally exposed port.

## Best Reuse

Use this note when you need to remember:

- the difference between `kubectl apply` and `kubectl get`
- how to confirm that a pod and service are actually running
- why `nodePort` is the external entry point
- why access may require a VM IP instead of `localhost`
