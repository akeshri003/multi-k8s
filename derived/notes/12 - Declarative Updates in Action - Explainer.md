# Declarative Updates in Action Explainer

## Source

- Transcript: `raw/transcripts/12 - Declarative Updates in Action.md`
- Wiki source page: [[12 - Declarative Updates in Action]]

## What This Lesson Is Trying To Prove

The previous lesson explained the rule for declarative updates:

- keep the object's identity fields stable
- change the config
- apply it again

This lesson proves that the workflow actually works in practice.

The question is:

If we change the pod manifest and apply it again, will Kubernetes update the old pod or make a second one?

The lesson's answer is:

it updates the existing pod.

## The Before State

The transcript starts by checking the current cluster and container state:

![[raw/assets/Screenshot 2026-04-19 at 3.04.42 PM.png]]

![[Screenshot 2026-04-19 at 3.05.02 PM.png]]

The commands shown are:

```bash
kubectl get pods
kubectl get services
docker ps
```

And the important things they show are:

- there is one pod named `client-pod`
- there is one `client-node-port` service
- Docker is currently running a container from `stephengrider/multi-client`

That gives us a clean before-and-after comparison.

## The Change Being Made

The lesson edits the original pod manifest and changes only the image:

```text
from: stephengrider/multi-client
to:   stephengrider/multi-worker
```

The transcript is careful to say that `multi-worker` might not be healthy yet because it expects Redis, but that does not matter for this lesson.

The goal here is narrower:

prove that Kubernetes can update the image on the existing pod.

## The Apply Step

The update is sent in with:

```bash
kubectl apply -f client-pod.yaml
```

And the returned message is:

```text
pod/client-pod configured
```

The lesson treats `configured` as the important clue. It means Kubernetes updated the configuration associated with that pod instead of telling us it created a brand-new object.

## The First Verification

After the apply, the lesson runs:

```bash
kubectl get pods
```

The key observation is:

- there is still only one pod
- it is still named `client-pod`
- its restart count has gone up

That matters because it suggests the existing pod was changed and restarted rather than duplicated.

## The Detailed Verification: kubectl describe

The biggest new command in this lesson is:

```bash
kubectl describe pod client-pod
```

This is introduced as the detailed inspection command for one specific object.

The lesson says `describe` is useful because it gives much richer information than `get`, especially:

- the current image
- container state
- restart history
- lifecycle events

The most important lines are:

```text
Image: stephengrider/multi-worker
Restart Count: 1
```

and the event history:

```text
Container client definition changed, will be restarted
Pulling image "stephengrider/multi-worker"
Successfully pulled image "stephengrider/multi-worker"
```

Those lines are the strongest proof in the lesson that Kubernetes really updated the existing pod's container definition.

## What This Actually Demonstrates

This lesson demonstrates three separate ideas at once.

### 1. Declarative updates are real, not just theory

You can change the config and apply it again. Kubernetes will use that desired state change to mutate the running system.

### 2. `kubectl describe` is the next verification tool after `kubectl get`

`get` answers:

- does the object exist?
- is it running?

`describe` answers:

- what image is it using?
- what happened recently?
- why did it restart?

### 3. The same object can survive with changed internals

The pod is still `client-pod`, but the container image inside it changed. That is the practical meaning of an in-place declarative update in this lesson's simplified model.

## Important Caveat

The lesson still uses the course's simplified rule:

- same `kind`
- same `name`
- therefore update the existing object

That is good enough for the current learning stage, but it is still a simplified teaching model. The vault's [[kubernetes-object-identity]] note is the more precise identity explanation.

## Best Reuse

Use this note when you want a simple explanation of:

- how declarative updates look in practice
- why `configured` matters after `kubectl apply`
- how to verify an update with `kubectl describe`
- what evidence shows that the existing pod changed instead of a new pod being created

