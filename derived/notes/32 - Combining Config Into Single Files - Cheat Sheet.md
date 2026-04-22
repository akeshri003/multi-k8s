# Combining Config Into Single Files Cheat Sheet

## Source

- Transcript: `raw/transcripts/32 - Combining Config Into Single Files.md`
- Wiki source page: [[32 - Combining Config Into Single Files]]

## Core Idea

You do not have to keep one Kubernetes object per file.

You can place multiple objects in one YAML file and separate them with `---`.

## Basic Pattern

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: server-deployment
spec:
  ...
---
apiVersion: v1
kind: Service
metadata:
  name: server-cluster-ip-service
spec:
  ...
```

## Example From The Lesson

- put the backend deployment and backend `ClusterIP` service into one file
- example filename: `server-config.yaml`

## Why Some People Like This

- related config stays together
- one file can represent one app component

## Why The Instructor Does Not Prefer It

- separate files make object lookup easier
- the filename can directly tell you where a specific object lives
- it is easier for another engineer to find one object quickly

## Instructor Recommendation

- keep one file per object
- continue using separate files through the rest of the course

## Retrieval Cues

- "`---` separates YAML documents"
- "multiple objects can live in one file"
- "valid approach, but not the course default"
- "separate files win on discoverability"
