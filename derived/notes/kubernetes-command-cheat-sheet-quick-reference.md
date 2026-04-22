# Kubernetes Command Cheat Sheet Quick Reference

## Main `kubectl` Loop

```bash
kubectl apply -f k8s
kubectl get pods
kubectl get services
kubectl get deployments
kubectl describe pod client-pod
kubectl logs <pod-name>
```

## Image Update Flow

```bash
docker build -t <docker-id>/multi-client:v5 .
docker push <docker-id>/multi-client:v5
kubectl set image deployment/client-deployment client=<docker-id>/multi-client:v5
```

## Secret Flow

```bash
kubectl create secret generic pgpassword --from-literal PGPASSWORD=postgres
kubectl get secrets
```

## Storage Checks

```bash
kubectl get pv
kubectl get pvc
kubectl get storageclass
kubectl describe storageclass
```

## GKE / Cloud Shell

```bash
gcloud config set project PROJECT_ID
gcloud config set compute zone COMPUTE_ZONE
gcloud container clusters get-credentials multi-cluster
```

## GitHub Actions Smoke / Deploy Checks

```bash
kubectl cluster-info
kubectl get nodes
kubectl rollout status deployment/client-deployment --timeout=300s
kubectl get ingress
```

## Ingress Controller

```bash
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

Full note: [[kubernetes-command-cheat-sheet]]
