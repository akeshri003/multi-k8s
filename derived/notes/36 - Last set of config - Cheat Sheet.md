# Last set of config Cheat Sheet

## Source

- Transcript: `raw/transcripts/36 - Last set of config.md`
- Wiki source page: [[36 - Last set of config]]

## Core Idea

This lesson adds the last repetitive deployment-plus-service pair: Postgres and its internal service.

## Main Postgres Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: postgres
  template:
    metadata:
      labels:
        component: postgres
    spec:
      containers:
        - name: postgres
          image: postgres
          ports:
            - containerPort: 5432
```

## Main Postgres Service YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: postgres-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: postgres
  ports:
    - port: 5432
      targetPort: 5432
```

## What Matters Most

- one Postgres replica
- public `postgres` image
- Postgres uses `5432`
- internal access through a `ClusterIP` service

## What Is Still Missing

- environment variables for server and worker
- the Postgres PVC

## Apply And Check

```bash
kubectl apply -f k8s
kubectl get pods
kubectl get services
```

## Retrieval Cues

- "last boring config pair"
- "`component: postgres`"
- "`5432` in and `5432` out"
- "PVC and env vars come next"
