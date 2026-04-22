# Object Types and API Versions Explainer

## Source

- Transcript: `raw/transcripts/5 - Object Types and API Versions.md`
- Wiki source page: [[5 - Object Types and API Versions]]

## The Main Confusion This Lesson Tries To Fix

Earlier lessons said that Kubernetes does not work like Docker Compose, but that is still abstract until you answer a basic question:

"If Kubernetes is not directly making containers from one config file, what is it making?"

This lesson's answer is: Kubernetes config files create `objects`.

## The Object Model

The course's first explanation is:

- you write config files
- you feed them to `kubectl`
- `kubectl` creates objects in the cluster

The screenshot below is the important mental model:

![[raw/assets/Screenshot 2026-04-17 at 7.32.11 PM.png]]

The point is that `object` is a category, not a single thing. Different object types do different jobs.

Examples shown in the lesson:

- `Pod`
- `Service`
- `ReplicaController`
- `StatefulSet`

## What `kind` Means

The `kind` field names the object type you want the manifest to create.

In the course examples:

```yaml
kind: Pod
```

means "create a Pod object," while:

```yaml
kind: Service
```

means "create a Service object."

That is the course's first real explanation of why Kubernetes needed multiple config files. The files are not just describing containers; they are creating different categories of cluster object.

## What `apiVersion` Means

The source explains `apiVersion` as a scope or pool of available object kinds.

The visual summary is:

![[raw/assets/Screenshot 2026-04-17 at 7.37.29 PM.png]]

The lesson's claim is:

- `apiVersion: v1` gives access to one set of object types
- `apiVersion: apps/v1` gives access to a different set

So in practice, the workflow is often:

1. decide which object kind you need
2. look up which API group/version that kind belongs to
3. use that `apiVersion` in the manifest

## The Two Example Manifests

### Pod manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: client-pod
  labels:
    component: web
spec:
  containers:
    - name: client
      image: stephengrider/multi-client
      ports:
        - containerPort: 3000
```

### Service manifest from the source block

```yaml
apiVersion: v1
kind: Service
metadata:
  name: client-node-port
spec:
  type: NodePort
  ports:
    - ports: 3050
      targetPort: 3000
      nodePort: 31515
  selector:
    component: web
```

## Important Caution

The source-provided service block appears to contain a likely typo:

```yaml
- ports: 3050
```

Inference:
The earlier walkthrough in [[4 - Adding Config files]] strongly suggests the intended field is:

```yaml
- port: 3050
```

So the useful reading is:

- preserve the source block as evidence
- treat the singular `port` form as the likely intended manifest

## How To Remember This As A Software Engineer

Docker Compose feels like "one file that describes services."

This lesson reframes Kubernetes as:

- a system with multiple object types
- each object type doing one role
- manifests acting like object declarations for cluster state

That is why Kubernetes feels more verbose at first. It is not just listing containers. It is modeling infrastructure responsibilities more explicitly.

## Best Reuse

Use this note when you need to remember:

- what `kind` does
- what `apiVersion` does
- why Kubernetes manifests create objects instead of directly mapping to a single Docker Compose service entry
