# Volumes vs Persistent Volumes Cheat Sheet

## Source

- Transcript: `raw/transcripts/35 - Volumes vs Persistent Volumes.md`
- Wiki source page: [[35 - Volumes vs Persistent Volumes]]

## Core Idea

Even though the title mentions volumes, this lesson is actually about creating Redis and its internal service.

## Main Redis Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: redis
  template:
    metadata:
      labels:
        component: redis
    spec:
      containers:
        - name: redis
          image: redis
          ports:
            - containerPort: 6379
```

## Main Redis Service YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: redis
  ports:
    - port: 6379
      targetPort: 6379
```

## What Matters Most

- one Redis replica
- public `redis` image
- Redis listens on `6379`
- internal access through `redis-cluster-ip-service`

## Apply And Check

```bash
kubectl apply -f k8s
kubectl get pods
kubectl get services
```

## Important Caveat

- the title says volumes, but the actual lesson is still about Redis config

## Retrieval Cues

- "Redis gets one deployment and one ClusterIP service"
- "`component: redis`"
- "6379 in and 6379 out"
- "title does not match lesson body yet"
