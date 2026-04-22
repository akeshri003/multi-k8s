---
title: Combining Config Into Single Files
type: source
status: active
created: 2026-04-19
updated: 2026-04-19
source_path: raw/transcripts/32 - Combining Config Into Single Files.md
source_date: 2026-04-19
tags:
  - kubernetes
  - config
  - yaml
  - workflow
  - organization
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/28 - The ClusterIP Config.md
  - wiki/sources/30 - Express API deployment config.md
  - wiki/sources/31 - ClusterIP for Express API.md
---

# Combining Config Into Single Files

## Metadata

- Source type: transcript with organizational workflow guidance and inline YAML separator example
- Transcript quality: strong enough to preserve the multi-document YAML pattern, the naming tradeoff, and the instructor's recommendation to keep one file per object
- Embedded visuals:
  - None explicitly provided in the transcript or newly added assets
- Derived notes:
  - `derived/notes/32 - Combining Config Into Single Files - Cheat Sheet.md`
  - `derived/notes/32 - Combining Config Into Single Files - Explainer.md`

## Summary

This source explains that Kubernetes config does not have to live in one file per object. Multiple objects can be combined into a single YAML file by placing `---` between documents. The lesson uses the backend deployment and backend `ClusterIP` service as the example pair that could be combined into one `server-config.yaml`-style file. But the instructor's actual recommendation is to keep separate files, because separate files make it easier for another engineer to quickly find the config for a specific object.

## Key Claims

- Kubernetes allows multiple object configs inside one file.
- There is no hard limit that forces one file per object.
- Multiple YAML documents are separated with `---`.
- A deployment and its related service can be colocated in one file if you prefer that organization style.
- The instructor prefers keeping one separate file per object.
- Separate files make it clearer how many objects exist in the cluster.
- Separate files also make it easier to find the config for a specific object by filename alone.
- Combined files are a valid option, but they require a stronger naming convention so people know which grouped objects live inside which file.

## Important Evidence Or Examples

- The transcript's concrete example is to combine the backend deployment and backend service into one file such as `server-config.yaml`.
- The multi-document YAML pattern is:

```yaml
# first object
apiVersion: apps/v1
kind: Deployment
metadata:
  name: server-deployment
...
---
# second object
apiVersion: v1
kind: Service
metadata:
  name: server-cluster-ip-service
...
```

- The lesson says there is no limit that forces only two objects per file; a single file could contain many related objects if you want.
- The transcript frames the main cost of combined files as discoverability:
  - another engineer may know they want the `client-cluster-ip-service`
  - but they may not know which grouped file contains that config
- The main benefit of separate files in the transcript is immediate lookup:
  - if the object is called `client-cluster-ip-service`
  - the matching config filename can make that obvious
- The instructor explicitly deletes the example combined file and continues the course with four separate files: two client files and two server files.

## Important Commands

- No shell command is central in this source.
- The lesson is about file organization rather than cluster operations.

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The lesson presents file organization as a matter of preference, but it does not discuss when a larger production repo might deliberately choose grouped manifests, templating, or other organizational layers.
- The source argues for separate files because of discoverability, but it does not show how that advice changes once the number of manifests grows well beyond the early course example.
- The lesson is about raw YAML file organization only; it does not yet connect this choice to later tooling such as Helm, Kustomize, or environment overlays.
