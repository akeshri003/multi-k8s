---
title: The ClusterIP Config
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/28 - The ClusterIP Config.md
source_date: 2026-04-19
tags:
  - kubernetes
  - service
  - clusterip
  - networking
  - selector
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/7 - Service Config Files in Depth.md
  - wiki/sources/27 - NodePort vs ClusterIP Services.md
---

# The ClusterIP Config

## Metadata

- Source type: transcript with embedded YAML and references to earlier service diagrams
- Transcript quality: strong enough to preserve both the full `ClusterIP` manifest and the explanation of how `port` and `targetPort` work without `nodePort`
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 8.21.07 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 10.12.39 PM.png]]
- Derived notes:
  - `derived/notes/28 - The ClusterIP Config - Cheat Sheet.md`
  - `derived/notes/28 - The ClusterIP Config - Explainer.md`

## Summary

This source turns the earlier `NodePort` versus `ClusterIP` distinction into an actual config file. It creates a new `Service` named `client-cluster-ip-service` for the `multi-client` deployment, keeps the selector on `component: web`, and shows that a `ClusterIP` service only needs `port` and `targetPort` because it is not meant to be reached directly from outside the cluster. The source also makes a useful practical point: `port` is the port other objects inside the cluster will use to talk to the service, while `targetPort` is the port on the target pod that traffic should reach.

## Key Claims

- A `ClusterIP` service is used to expose a set of pods to other objects inside the cluster.
- The new frontend service should target the `multi-client` pods created by the client deployment.
- The service selector should stay aligned with the deployment's pod label, `component: web`.
- `ClusterIP` service config looks very similar to `NodePort` service config.
- A `ClusterIP` service does not use a `nodePort` field.
- `port` is the port other objects inside the cluster use to access the service.
- `targetPort` is the port on the target pod/container that traffic should be forwarded to.
- `port` and `targetPort` can differ, but this lesson keeps them the same at `3000`.

## Important Evidence Or Examples

- The source includes this service manifest:

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

- The transcript explains that the new service should select the same pods created by the client deployment:

```yaml
selector:
  component: web
```

- The lesson contrasts this service with the earlier `NodePort` version:
  - `NodePort` needed `port`, `targetPort`, and `nodePort`
  - `ClusterIP` only needs `port` and `targetPort`
- The source explicitly says the service is not addressable from the outside world, which is why there is no `nodePort`.
- The source briefly notes that port remapping is possible, but the chosen config keeps both values at `3000` because there is no good reason to mismatch them in this example.
- The transcript appears to garble the intended file extension as `client cluster IP service.emo`; the surrounding explanation makes `client-cluster-ip-service.yaml` the intended filename.

## Important Commands

- No shell command is central in this source.
- The lesson is mainly about writing the `ClusterIP` service manifest correctly.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- This lesson creates the frontend `ClusterIP` service, but it still stops short of showing the actual `Ingress` manifest that will route outside traffic into it.
- The source explains `port` and `targetPort` well, but it still does not show how DNS naming works between services inside the cluster.
- The lesson keeps `port` and `targetPort` equal for simplicity, so the broader reasons to deliberately remap ports are still left unexplored.
