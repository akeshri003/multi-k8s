# Unique Deployment Images Cheat Sheet

## Source

- Transcript: `raw/transcripts/63 - Unique Deployment Images.md`
- Wiki source page: [[63 - Unique Deployment Images]]

## First Draft Of `deploy.sh`

- build `multi-client`
- build `multi-server`
- build `multi-worker`
- push all three images
- `kubectl apply -f k8s`
- `kubectl set image` on each deployment

## Core Problem

- this first draft still relies on the implicit `latest` tag
- if the deployment already thinks it is running that image reference, no rollout happens

## Example Commands

```bash
docker build -t DOCKER_ID/multi-client -f ./client/Dockerfile ./client
docker build -t DOCKER_ID/multi-server -f ./server/Dockerfile ./server
docker build -t DOCKER_ID/multi-worker -f ./worker/Dockerfile ./worker

docker push DOCKER_ID/multi-client
docker push DOCKER_ID/multi-server
docker push DOCKER_ID/multi-worker

kubectl apply -f k8s
```

## What This Lesson Sets Up

- the next lesson adds unique image tags so `kubectl set image` actually changes something

See also: [[64 - Unique Tags for Built Image]]
