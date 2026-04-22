# Object Types and API Versions Cheat Sheet

## Source

- Transcript: `raw/transcripts/5 - Object Types and API Versions.md`
- Assets:
  - ![[raw/assets/Screenshot 2026-04-17 at 7.31.05 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 7.32.11 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 7.37.29 PM.png]]
- Wiki source page: [[5 - Object Types and API Versions]]

## Core Ideas

- Kubernetes config files create `objects`.
- `kind` says what object type you want.
- `apiVersion` limits which object types are available in that config file.

## First Two Example Objects

- `Pod`: used here to run a container
- `Service`: used here to set up networking

## Source Blocks

### `client-pod.yaml`

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

### `client-node-port.yaml`

As captured in the source:

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

Probable correction inferred from the earlier walkthrough in [[4 - Adding Config files]]:

```yaml
ports:
  - port: 3050
    targetPort: 3000
    nodePort: 31515
```

## Retrieval Cues

- "config file creates objects"
- "`kind` picks the object type"
- "`apiVersion` scopes available kinds"
- "`v1` gives access to Pod and Service in these examples"
