# Service Config Files in Depth Explainer

## Source

- Transcript: `raw/transcripts/7 - Service Config Files in Depth.md`
- Wiki source page: [[7 - Service Config Files in Depth]]

## What This Lesson Adds

Earlier notes introduced `Service` as a Kubernetes object kind and showed a first `NodePort` manifest. This lesson explains what that manifest is actually doing.

Its core job is to explain:

- why the `Service` object exists
- why `NodePort` is a subtype
- how the selector matches the target pod
- what `port`, `targetPort`, and `nodePort` each mean

## Services Versus Pods

The lesson contrasts the two object types very directly:

![[raw/assets/Pasted image 20260417205856.png]]

The summary is:

- `Pods` run one or more closely related containers
- `Services` set up networking in the cluster

That is a good first division of labor to remember.

## NodePort In Particular

The service subtype diagram is the next key picture:

![[raw/assets/Screenshot 2026-04-17 at 9.00.48 PM.png]]

The source says:

- `ClusterIP`
- `NodePort`
- `LoadBalancer`
- `Ingress`

and focuses only on `NodePort` for now.

The practical claim is:

- `NodePort` exposes a container to the outside world
- it is mainly for development, not the normal production path

## The Service Path

This diagram is the one that makes the request flow concrete:

![[raw/assets/Screenshot 2026-04-17 at 9.05.25 PM.png]]

The lesson's model is:

1. the browser sends a request
2. traffic reaches the node
3. `kube-proxy` handles external entry to the node
4. the `NodePort` service receives that traffic
5. the service forwards traffic to port `3000` on the target pod/container

This is the first time the networking path starts to look like a real system rather than just YAML.

## Why Labels And Selectors Matter

This is the best diagram in the lesson for understanding the relation between the pod and the service:

![[raw/assets/Screenshot 2026-04-17 at 9.11.17 PM.png]]

The key mechanism is:

- the pod has a label
- the service has a selector
- the selector finds pods with matching labels

So the earlier pairing:

```yaml
labels:
  component: web
```

and

```yaml
selector:
  component: web
```

is how the service knows which pod to target.

## The Three Ports

The lesson then separates the three port fields:

![[raw/assets/Screenshot 2026-04-17 at 10.12.39 PM.png]]

Useful reading:

- `port`
  the service port other pods can use inside the cluster
- `targetPort`
  the port on the pod/container that traffic should reach
- `nodePort`
  the externally exposed port on the node for development access

That yields the readable version of the service manifest:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: client-node-port
spec:
  type: NodePort
  ports:
    - port: 3050
      targetPort: 3000
      nodePort: 31515
  selector:
    component: web
```

## Important Caution

The pasted YAML in the transcript again appears to contain a likely field-name typo around `ports` versus `port`. For readable reuse, the normalized singular `port` form is the safer interpretation, but the source-page record keeps the distinction explicit.

## Best Reuse

Use this note when you need to remember:

- what a `Service` does
- why `NodePort` is a development-oriented service type
- how labels and selectors connect services to pods
- what each of the three service port fields means
