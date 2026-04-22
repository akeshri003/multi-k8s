# Updating Existing Objects Cheat Sheet

## Source

- Transcript: `raw/transcripts/11 - Updating Existing Objects.md`
- Wiki source page: [[11 - Updating Existing Objects]]
- Key visuals:
  - ![[raw/assets/Screenshot 2026-04-19 at 2.46.24 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-19 at 2.46.52 PM.png]]
  - ![[Screenshot 2026-04-19 at 2.49.24 PM.png]]

## Core Idea

To update an existing Kubernetes object declaratively:

1. find the original config file
2. keep the object `name` the same
3. keep the object `kind` the same
4. change the desired field
5. run `kubectl apply -f ...` again

## Example In This Lesson

The old pod used:

```text
stephengrider/multi-client
```

The new goal is:

```text
stephengrider/multi-worker
```

## Imperative Versus Declarative

- imperative
  list current pods, find the right pod, work out the exact update command yourself
- declarative
  edit the original config file and let Kubernetes apply the change

## Matching Rule Taught Here

The lesson's practical rule is:

- same `kind`
- same `metadata.name`

means Kubernetes treats the file as an update to the existing object.

If the name changes, Kubernetes treats it as a new object.

## Example Manifest

This is a normalized illustration of the change the transcript describes:

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
      image: stephengrider/multi-worker
      ports:
        - containerPort: 3000
```

## Retrieval Cues

- "edit the original config file"
- "leave name and kind the same"
- "change the image and re-apply"
- "new name means new object"

