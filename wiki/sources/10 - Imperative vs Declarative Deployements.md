---
title: Imperative vs Declarative Deployements
type: source
status: active
created: 2026-04-17
updated: 2026-04-17
source_path: raw/transcripts/10 - Imperative vs Declarative Deployements.md
source_date: 2026-04-17
tags:
  - kubernetes
  - deployment
  - declarative
  - imperative
  - desired-state
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/9 - Entire Deployment Flow.md
---

# Imperative vs Declarative Deployements

## Metadata

- Source type: transcript with embedded screenshots and conceptual diagrams
- Transcript quality: strong enough to preserve both the conceptual comparison and the concrete scaling and version-update examples
- Embedded visuals:
  - ![[raw/assets/Screenshot 2026-04-17 at 11.26.05 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.30.52 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.32.10 PM.png]]
  - ![[raw/assets/Pasted image 20260417233416.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.35.30 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.37.45 PM.png]]
  - ![[raw/assets/Screenshot 2026-04-17 at 11.40.11 PM.png]]
- Derived notes:
  - `derived/notes/10 - Imperative vs Declarative Deployements - Cheat Sheet.md`
  - `derived/notes/10 - Imperative vs Declarative Deployements - Explainer.md`

## Summary

This source turns the earlier "desired state" discussion into a direct comparison between two deployment styles. In an imperative approach, the operator inspects the current cluster state, computes the difference from the desired state, and issues specific commands such as adding, removing, or updating individual workloads. In a declarative approach, the operator edits a configuration file to describe the desired result and sends that file to Kubernetes, letting the master figure out the migration steps. The transcript strongly argues that real Kubernetes usage, especially in production, should favor the declarative model.

## Key Claims

- Kubernetes is fundamentally about deploying containerized applications, even though users interact with higher-level objects such as pods and services.
- A node is an individual machine or virtual machine that runs containers or Kubernetes objects.
- The master manages what happens across nodes and decides placement by default unless specific scheduling constraints are added.
- Kubernetes does not build the image during deployment; it pulls the image from Docker Hub.
- In the lessons so far, developers interact with Kubernetes by updating the desired state through configuration files.
- The master continuously works to make actual cluster state match desired state.
- An imperative deployment style requires the operator to inspect current state and compute a migration path manually.
- A declarative deployment style requires the operator to describe the desired end state and let Kubernetes determine how to get there.
- Kubernetes supports both styles through `kubectl`, but the transcript argues that declarative workflows are the right default for real production use.

## Important Evidence Or Examples

- The source begins by restating the previous lesson's main ideas: Kubernetes runs containerized apps, nodes execute workloads, the master manages cluster behavior, Docker Hub supplies images, and the master decides placement unless instructed otherwise.
- One example of imperative deployment starts with four running containers when the desired state is three. The operator must inspect the current state, notice the mismatch, and issue a command to remove one container.
- A second imperative example asks for a version upgrade from an older `multi-worker` image version to `1.23`. The operator must find the correct set of running containers and work out how to update only those targets.
- The declarative alternative is presented as editing the configuration file's image tag and sending that updated file back to Kubernetes.
- The transcript says the master then updates its internal responsibility list, finds pods that are not using the desired version, removes outdated pods, and creates new pods with the correct version.
- The lesson repeatedly frames declarative deployment as the core Kubernetes mental model: update config, send it to the master, let Kubernetes handle the migration.
- The transcript warns that many blog posts and docs snippets show imperative commands, but says the course will deliberately prefer declarative workflows throughout.

## Important Commands Or Config Patterns

- The transcript does not center on one specific `kubectl` command here, but it repeatedly refers to the same overall pattern:

```text
1. update the configuration file
2. send the updated configuration to Kubernetes
3. let Kubernetes reconcile actual state to match desired state
```

- The concrete declarative example is updating the image tag in a configuration file, conceptually from an older version to:

```text
multi-worker:1.23
```

## Related Topics

- [[kubernetes]]

## Related Entities

- None yet.

## Tensions Or Open Questions

- The source strongly favors declarative deployment, but it also acknowledges that `kubectl` exposes imperative commands, so future notes should clarify when imperative commands are acceptable for debugging or local experimentation.
- The transcript still uses older `master` terminology and a simplified control model, so later sources may need to map this explanation onto modern Kubernetes terms and objects.

