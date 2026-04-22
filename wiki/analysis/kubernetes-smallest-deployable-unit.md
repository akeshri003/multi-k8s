---
title: Kubernetes Smallest Deployable Unit
type: analysis
status: active
created: 2026-04-19
updated: 2026-04-19
tags:
  - kubernetes
  - pod
  - object-model
  - deployment
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/5 - Object Types and API Versions.md
  - wiki/sources/6 - Running Container in Pods.md
  - wiki/analysis/kubernetes-object-identity.md
---

# Kubernetes Smallest Deployable Unit

## Question

What is the smallest deployable unit in Kubernetes? Is it any type of object, or is it `Pod` specifically?

## Answer

It is `Pod` specifically, not "any object."

In Kubernetes:

- `object` is the broad category
- `Pod` is one object kind
- the smallest deployable unit is a `Pod`

So:

- `Service` is an object, but not a deployable workload unit
- `ConfigMap` is an object, but not a deployable workload unit
- `Pod` is the first object kind that actually runs container(s)

That matches the current vault notes in [[6 - Running Container in Pods]] and [[kubernetes]]: Kubernetes does not deploy naked standalone containers; it deploys them inside a `Pod`.

## Simple Memory Hook

- smallest Kubernetes API record: not a useful framing, because many objects are config or control objects
- smallest workload unit that actually gets scheduled and run: `Pod`

So if the real question is:

"What is the smallest deployable thing that runs application containers?"

the answer is:

```text
Pod
```

## Related Pages

- [[6 - Running Container in Pods]]
- [[5 - Object Types and API Versions]]
- [[kubernetes-object-identity]]

