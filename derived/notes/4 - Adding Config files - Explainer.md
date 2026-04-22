# Adding Config Files Explainer

## Source

- Transcript: `raw/transcripts/4 - Adding Config files.md`
- Wiki source page: [[4 - Adding Config files]]

## What This Lesson Actually Does

This source is the first place where the Kubernetes material stops being purely conceptual and becomes concrete. The lesson takes the earlier idea of "Kubernetes uses multiple config files for objects" and turns it into actual YAML.

The practical move is simple:

1. make sure the image already exists on Docker Hub
2. write one manifest for the workload
3. write one manifest for networking

## The Two Files

### Workload file

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

This file creates the first container-running object in the course: a `Pod`.

### Networking file

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

This second file creates a `Service` that makes the pod reachable from outside the cluster.

## How To Think About It

If you are coming from Docker Compose, this lesson is the first real break from the "one file describes everything" model.

- Compose bundled image build, container declaration, and port mapping into one service entry.
- Kubernetes splits those responsibilities across objects and files.

Even before the course explains `kind` and `apiVersion` in detail, this lesson already shows the working pattern:

- one object for the workload
- one object for networking

## Important Inference From The YAML

The two manifests are meant to connect through the shared label:

```yaml
labels:
  component: web
```

and

```yaml
selector:
  component: web
```

This is the bridge between the Pod and the Service. The transcript does not unpack that relationship yet, but the manifests clearly set it up.

## Why The Instructor Keeps Warning About Indentation

YAML is structure-by-whitespace. That means formatting mistakes are not cosmetic.

In this lesson, indentation controls:

- whether `labels` is nested under `metadata`
- whether `containers` is nested under `spec`
- whether the port fields belong to the service port entry

That is why the transcript repeatedly warns to triple-check spacing.

## Best Reuse

Use this note when you need the first concrete Kubernetes example:

- a Pod manifest
- a NodePort Service manifest
- the first example of image plus object plus networking working together
