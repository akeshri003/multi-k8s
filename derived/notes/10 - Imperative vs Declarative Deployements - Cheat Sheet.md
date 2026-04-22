# Imperative vs Declarative Deployements Cheat Sheet

## Source

- Transcript: `raw/transcripts/10 - Imperative vs Declarative Deployements.md`
- Wiki source page: [[10 - Imperative vs Declarative Deployements]]
- Key visuals:
  - ![[raw/assets/Screenshot 2026-04-17 at 11.26.05 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.30.52 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.32.10 PM.png]]
  - ![[raw/assets/Pasted image 20260417233416.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.35.30 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.37.45 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.40.11 PM.png]]

## Core Idea

Kubernetes can be used in two styles:

- imperative
- declarative

This lesson argues that declarative is the right default.

## Imperative Style

You work out the change yourself.

That means you:

1. inspect the current state
2. compare it to the desired state
3. decide the exact actions to run
4. issue commands like add, remove, or update

## Declarative Style

You describe the end state you want.

That means you:

1. edit the config file
2. describe the desired state there
3. send that file to Kubernetes
4. let Kubernetes figure out the migration steps

## Simple Example

If you want:

```text
3 containers
```

but currently have:

```text
4 containers
```

then in an imperative flow you must notice that gap and decide to remove one container yourself.

## Version Update Example

If you want all `multi-worker` pods to run:

```text
multi-worker:1.23
```

then declarative Kubernetes says:

- update the config file
- send it back to the master
- let Kubernetes replace outdated pods automatically

## Key Takeaways

- Kubernetes is about deploying containerized apps.
- Nodes run workloads.
- The master decides placement by default.
- Docker Hub supplies the image.
- Declarative deployment means "here is the state I want."
- Imperative deployment means "here are the exact commands to run."
- The course prefers declarative workflows for production-style Kubernetes usage.

## Retrieval Cues

- "imperative means manual migration plan"
- "declarative means desired state in config"
- "edit config, send to Kubernetes, let it reconcile"
- "production default is declarative"

