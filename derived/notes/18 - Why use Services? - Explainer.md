# Why use Services? Explainer

## Source

- Transcript: `raw/transcripts/18 - Why use Services?.md`
- Wiki source page: [[18 - Why use Services?]]

## What Question This Lesson Answers

By this point in the course, you already have:

- a deployment creating pods
- a service exposing the application

So the natural question is:

why do we still need the service at all?

Why not just connect straight to the pod that is running the app?

This lesson answers that directly.

## Step 1: Look At The Pod In Wide Format

The lesson starts by asking for:

```bash
kubectl get pods -o wide
```

The recorded output is:

```text
NAME                                 READY   STATUS    RESTARTS   AGE     IP          NODE             NOMINATED NODE   READINESS GATES
client-deployment-576bc76988-mp7wn   1/1     Running   0          6m50s   10.1.0.15   docker-desktop   <none>           <none>
```

The new thing to notice is the `IP` column.

That column shows that the pod itself has an address.

At first, that can make it feel like a service is unnecessary. You might think:

if the pod already has an IP, why not just open that?

## Step 2: Why Pod IP Is A Bad User-Facing Address

The lesson's answer is:

because a pod is not a stable thing to point your browser at.

Pods can be:

- deleted
- recreated
- replaced after a config change
- replaced after a crash

When that happens, the replacement pod can get a different IP address.

So today you might have one pod at:

```text
172.0.0.1:3000
```

and later the old pod might disappear and a replacement pod might show up at:

```text
172.0.0.2:3000
```

If you were connecting directly to the pod, you would have to keep discovering and updating that address yourself.

That would be annoying even in development, and it would be a bad design for any serious system.

## Step 3: What The Service Actually Does

The service gives you a stable front door.

Instead of remembering whichever pod IP happens to exist right now, you connect to the service entry point.

In this lesson, the path is described as:

```bash
minikube ip
```

followed by:

```text
<node-ip>:31515
```

The important thing is not the exact local runtime command. The important thing is the routing idea:

browser -> node access point -> service -> current matching pod

That means the browser keeps talking to the same stable entry point, while Kubernetes and the service take care of which pod is actually behind it.

## Step 4: How The Service Knows Which Pod To Use

The lesson ties this back to the earlier service config.

The service has a selector.

The selector used in this course example is:

```yaml
selector:
  component: web
```

That means the service keeps looking for pods with that label and routes traffic to them.

So the service is not hard-coded to one specific pod IP.

It is attached to a set of pods defined by labels.

That is the key design idea.

## What The Diagrams Add

The screenshots are useful because they show two different mental models.

The first model shows a stable route:

- browser sends traffic in
- `kube-proxy` forwards it
- the service receives it on `31515`
- the pod receives it on `3000`

The later diagrams show why direct pod access breaks down:

- first pod exists at one IP
- then a new pod appears at another IP
- then the old pod disappears

That visual change is the reason services exist.

## Important Caveat

The spoken explanation uses the older `minikube` VM framing and says you should use `minikube ip`.

But the recorded `kubectl get pods -o wide` output shows the node as `docker-desktop`.

So you should treat the runtime-specific connection path carefully.

The durable lesson is not "always use this one exact local command."

The durable lesson is:

services give you a stable access layer over pods whose identities and IPs can change.

## Best Reuse

Use this note when you want a simple explanation of:

- why pod IPs are not stable access points
- why deployments make direct pod access even less reliable
- how a service hides pod churn
- why selectors matter for service routing
