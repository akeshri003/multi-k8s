---
title: Kubernetes Command Cheat Sheet
type: analysis
status: active
created: 2026-04-22
updated: 2026-04-22
tags:
  - kubernetes
  - commands
  - cheat-sheet
  - docker
  - gke
  - github-actions
related:
  - wiki/topics/kubernetes.md
  - wiki/analysis/github-actions-gke-deployment-flow.md
  - wiki/analysis/kubernetes-cloud-deployment-secret-management.md
  - wiki/sources/8 - Connecting to Running Containers.md
  - wiki/sources/23 - Imperatively Updating a Deployment's Image.md
  - wiki/sources/65 - Updating the Deployment Script.md
  - wiki/sources/66 - Configuring the gcloud CLI on the cloud console.md
  - wiki/sources/67 - Helm and its setup.md
---

# Kubernetes Command Cheat Sheet

## Question

What are the main shell commands learned so far across the ingested Kubernetes, Docker, GKE, and deployment notes?

## Answer

This page is a cumulative cheat sheet of the command patterns learned so far. It groups commands by use case and keeps the concrete forms already seen in the source material. It is intentionally shell-command focused, so it does not try to repeat all YAML manifest fields or every GitHub Action `uses:` step.

## Core `kubectl` Apply And Inspect Commands

Sources: [[8 - Connecting to Running Containers]], [[12 - Declarative Updates in Action]], [[17 - Applying a Deployment]], [[19 - Scaling and Chaning Deployments]], [[29 - Applying Multiple Files with Kubectl]], [[34 - Reapplying Batch of Config Files]], [[45 - Applying a PVC]]

```bash
kubectl apply -f client-pod.yaml
kubectl apply -f client-node-port.yaml
kubectl apply -f client-deployment.yaml
kubectl apply -f k8s

kubectl get pods
kubectl get services
kubectl get deployments
kubectl get pods -o wide

kubectl describe pod client-pod
kubectl describe pods

kubectl logs <pod-name>
kubectl logs server-deployment-56685c959c-99zpc
```

Use these for the basic loop:

- apply manifests
- check what exists
- inspect a specific object in detail
- read pod logs when `Running` is not enough

## Delete And Replace Commands

Sources: [[17 - Applying a Deployment]], [[29 - Applying Multiple Files with Kubectl]]

```bash
kubectl delete -f client-pod.yaml
kubectl delete deployment client-deployment
kubectl delete service client-node-port
```

These are the imperative cleanup commands used before replacing older objects with newer deployment or service definitions.

## Deployment Image Update Commands

Sources: [[22 - Triggering Deployment Updates]], [[23 - Imperatively Updating a Deployment's Image]], [[63 - Unique Deployment Images]], [[65 - Updating the Deployment Script]], [[github-actions-gke-deployment-flow]]

```bash
kubectl set image deployment/client-deployment client=<docker-id>/multi-client:v5
kubectl set image deployment/server-deployment server=DOCKER_ID/multi-server:$SHA
kubectl set image deployment/worker-deployment worker=DOCKER_ID/multi-worker:$SHA
```

This is the key imperative command used when `kubectl apply -f ...` alone does not pick up newly pushed images.

## Rollout And Cluster State Commands

Sources: [[2026-04-22-mental-model-of-github-actions-gke-deployment]], current repo deploy workflow, and the recent deployment notes

```bash
kubectl rollout status deployment/client-deployment --timeout=300s
kubectl rollout status deployment/server-deployment --timeout=300s
kubectl rollout status deployment/worker-deployment --timeout=300s

kubectl cluster-info
kubectl get nodes
kubectl get ingress
```

These are the commands used after cloud auth succeeds to prove the cluster is reachable and the deployments are actually converging.

## Secret Commands

Sources: [[48 - Creating an Encoded Secret]], [[49 - Postgres Environment Variable Fix]], [[66 - Configuring the gcloud CLI on the cloud console]], [[2026-04-22-managing-secrets-for-cloud-deployment]], [[kubernetes-cloud-deployment-secret-management]]

```bash
kubectl create secret generic pgpassword --from-literal PGPASSWORD=postgres
kubectl get secrets

kubectl create secret generic pgpassword \
  --from-literal PGPASSWORD='your-real-password-here'

kubectl create secret generic pgpassword \
  --from-literal PGPASSWORD="$PG_PASSWORD" \
  --dry-run=client -o yaml | kubectl apply -f -
```

The first command is the course-style baseline.

The last command is the more reproducible GitHub Actions pattern for "create or update" rather than "create once."

## Storage Commands

Sources: [[43 - Where does Kubernetes Allocate Persistent Volumes]], [[45 - Applying a PVC]]

```bash
kubectl get storageclass
kubectl describe storageclass

kubectl get pv
kubectl get pvc
```

These are the inspection commands used to understand how storage is provisioned and whether a PVC is actually bound.

## Docker And Docker Compose Commands

Sources: [[9 - Entire Deployment Flow]], [[21 - Rebuilding the Client Image]], [[23 - Imperatively Updating a Deployment's Image]], [[25 - A Quick Checkpoint]], [[63 - Unique Deployment Images]], [[65 - Updating the Deployment Script]], [[github-actions-gke-deployment-flow]]

```bash
docker ps
docker kill <container-id>

docker-compose up --build
docker-compose up

docker build -t <docker-id>/multi-client .
docker build -t <docker-id>/multi-client:v5 .

docker build -t DOCKER_ID/multi-client -f ./client/Dockerfile ./client
docker build -t DOCKER_ID/multi-server -f ./server/Dockerfile ./server
docker build -t DOCKER_ID/multi-worker -f ./worker/Dockerfile ./worker

docker build -t DOCKER_ID/multi-client:latest -t DOCKER_ID/multi-client:$SHA -f ./client/Dockerfile ./client
docker build -t DOCKER_ID/multi-server:latest -t DOCKER_ID/multi-server:$SHA -f ./server/Dockerfile ./server
docker build -t DOCKER_ID/multi-worker:latest -t DOCKER_ID/multi-worker:$SHA -f ./worker/Dockerfile ./worker

docker push <docker-id>/multi-client
docker push <docker-id>/multi-client:v5

docker push DOCKER_ID/multi-client
docker push DOCKER_ID/multi-server
docker push DOCKER_ID/multi-worker

docker push DOCKER_ID/multi-client:latest
docker push DOCKER_ID/multi-client:$SHA
docker push DOCKER_ID/multi-server:latest
docker push DOCKER_ID/multi-server:$SHA
docker push DOCKER_ID/multi-worker:latest
docker push DOCKER_ID/multi-worker:$SHA

docker run --rm -e CI=true multi-client-test npm test
```

The main progression is:

- local Docker / Compose sanity checks
- manual image rebuild and push
- version-tagged rebuild and push
- CI-driven dual-tag image build and push

## Git Commands For Image Versioning

Sources: [[64 - Unique Tags for Built Image]], [[65 - Updating the Deployment Script]]

```bash
git rev-parse HEAD
git log
```

These commands are what turn image tags into traceable deployment versions.

## Local Kubernetes Runtime Commands

Sources: [[2 - Kubernetes in Development and Production]], [[8 - Connecting to Running Containers]], [[18 - Why use Services?]], [[20 - Updating Deployment Images]], [[23 - Imperatively Updating a Deployment's Image]]

```bash
minikube ip
```

This is the command repeatedly used in the older `minikube`-style local access flow to get the node IP for a `NodePort` service.

## Historical GCP / Travis Commands

Sources: [[61 - Installing google cloud sdk]], [[62 - Service Account Steps for GCP]]

```bash
curl https://sdk.cloud.google.com | bash > /dev/null
source $HOME/google-cloud-sdk/path.bash.inc
gcloud components update kubectl
gcloud auth activate-service-account --key-file service-account.json
```

These are historically important because the course used Travis CI, but the wiki now prefers the GitHub Actions-based replacement path instead.

## Current GKE Targeting Commands

Sources: [[66 - Configuring the gcloud CLI on the cloud console]]

```bash
gcloud config set project PROJECT_ID
gcloud config set compute zone COMPUTE_ZONE
gcloud container clusters get-credentials multi-cluster
```

Use these when Cloud Shell or a local terminal needs to target the live cluster before running `kubectl`.

## Ingress / Helm Commands

Sources: [[56 - Ingress Nginx installation and setup]], [[67 - Helm and its setup]], [[kubernetes-modern-ingress-format]]

```bash
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace

kubectl get service ingress-nginx-controller --namespace=ingress-nginx
```

The older Docker Desktop note points to the official ingress-nginx quick-start `kubectl apply` script, but the exact command is not preserved locally. The Helm command above is the current GKE-oriented command captured in the wiki.

## Current Recommended Cloud Deploy Sequence

Sources: [[github-actions-gke-deployment-flow]], [[kubernetes-cloud-deployment-secret-management]]

```bash
kubectl cluster-info
kubectl get nodes
kubectl apply -f k8s
kubectl set image deployment/client-deployment client=YOUR_DOCKER_ID/multi-client:${GITHUB_SHA}
kubectl set image deployment/server-deployment server=YOUR_DOCKER_ID/multi-server:${GITHUB_SHA}
kubectl set image deployment/worker-deployment worker=YOUR_DOCKER_ID/multi-worker:${GITHUB_SHA}
```

This is the cumulative operational pattern the wiki now supports:

1. prove GitHub Actions can reach GKE
2. build and push versioned images
3. apply manifests
4. update deployments to the exact pushed tags
5. verify rollout

## Uncertainties

- This page captures command patterns learned so far, not every YAML snippet or every vendor-specific option.
- Some commands are historical course commands, especially the Travis and service-account-key flow, and the wiki now prefers modern GitHub Actions based replacements where those exist.
- The wiki still does not contain the final settled `deploy.yaml` for this repo, so the cloud deploy sequence remains the current recommended pattern rather than the final committed implementation.

## Related Pages

- [[kubernetes]]
- [[github-actions-gke-deployment-flow]]
- [[kubernetes-cloud-deployment-secret-management]]
- [[kubernetes-modern-ingress-format]]
- [[multi-k8s-github-actions-files-and-flow]]
