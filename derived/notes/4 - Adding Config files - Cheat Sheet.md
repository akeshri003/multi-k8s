# Adding Config Files Cheat Sheet

## Source

- Transcript: `raw/transcripts/4 - Adding Config files.md`
- Wiki source page: [[4 - Adding Config files]]

## Goal

Get the `multi-client` image running on a local Kubernetes cluster.

## Main Move

Translate the earlier Docker Compose mental model into two Kubernetes manifests:

- one file to create the workload
- one file to expose it through networking

## Files To Create

### `client-pod.yml`

Normalized from the spoken walkthrough:

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

### `client-node-port.yml`

Normalized from the spoken walkthrough:

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

## What To Remember

- `Pod` is the first object used to run the client container.
- `Service` of type `NodePort` is used to expose it.
- The image is expected to already exist on Docker Hub.
- YAML indentation matters. A small spacing mistake can break the manifest.

## Retrieval Cues

- "first Pod manifest"
- "first NodePort service"
- "image already on Docker Hub"
- "two files: workload and networking"
