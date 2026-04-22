# Service Config Files in Depth Cheat Sheet

## Source

- Transcript: `raw/transcripts/7 - Service Config Files in Depth.md`
- Wiki source page: [[7 - Service Config Files in Depth]]
- Key visuals:
  - ![[raw/assets/Pasted image 20260417205856.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 9.00.48 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 9.05.25 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 9.11.17 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 10.12.39 PM.png]]

## Core Idea

`Service` objects set up networking in Kubernetes.

The lesson focuses on the `NodePort` subtype, which is mainly useful for development.

## Main YAML

Normalized for readability:

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

## What Each Part Means

- `type: NodePort`
  Exposes the target workload to the outside world for development access.
- `selector: component: web`
  Finds pods with the matching label.
- `port`
  Internal service port that other pods can use.
- `targetPort`
  Actual port on the target pod/container.
- `nodePort`
  External port you hit from the browser on the node.

## Retrieval Cues

- "Service sets up networking"
- "NodePort is mainly for dev"
- "selector finds matching labels"
- "port vs targetPort vs nodePort"
