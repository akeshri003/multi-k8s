---
title: Kubernetes Object Identity
type: analysis
status: active
created: 2026-04-19
updated: 2026-04-19
tags:
  - kubernetes
  - object-model
  - pod
  - metadata
  - identity
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/5 - Object Types and API Versions.md
  - wiki/sources/6 - Running Container in Pods.md
---

# Kubernetes Object Identity

## Question

In the context of Kubernetes, How do we define a unique object? What is the signature of a pod object?

## Answer
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: client-pod
```

means "create an object whose kind is `Pod`."

## Practical Identity

For uniqueness, the practical identity of an object is usually:

- `kind`
- `metadata.name`
- `metadata.namespace` for namespaced objects

So a pod is usually identified like:

```text
(Pod, client-pod, default)
```

Two important details:

- `name` alone is not enough, because a `Service` and a `Pod` can share the same name in the same namespace.
- the truly permanent identity is `metadata.uid`, which Kubernetes assigns internally. Names can be reused after deletion; `uid` cannot.

## Two Useful Layers

If you want the "signature" of a pod object, it helps to think in two layers.

### 1. Type signature

```yaml
apiVersion: v1
kind: Pod
```

This tells Kubernetes what sort of object this is.

### 2. Instance identity

```yaml
metadata:
  name: client-pod
  namespace: default
```

This identifies the particular pod instance in normal day-to-day use.

Then the rest:

```yaml
spec:
  ...
```

defines what that pod should do, not who it is.

## Short Version

- `object` is the general term
- `Pod` is one object kind
- a pod is usually identified by `kind + namespace + name`
- its immutable internal identity is `metadata.uid`

## Evidence In This Vault

- [[5 - Object Types and API Versions]] explains that manifests create Kubernetes objects and that `kind` picks the object type.
- [[6 - Running Container in Pods]] explains that `Pod` is the smallest deployable unit, which makes it one important object kind but not the only one.

