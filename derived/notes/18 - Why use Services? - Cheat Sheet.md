# Why use Services? Cheat Sheet

## Source

- Transcript: `raw/transcripts/18 - Why use Services?.md`
- Wiki source page: [[18 - Why use Services?]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-19 at 3.58.25 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 3.58.34 PM.png]]
- ![[raw/assets/Screenshot 2026-04-19 at 3.59.00 PM.png]]

## Core Idea

You do not connect to a pod directly because pods are not stable targets.

- each pod gets its own IP
- that IP is internal to the cluster/node environment
- the pod can be deleted and recreated
- the replacement pod can get a different IP

A `Service` gives you one stable way to reach whichever pod currently matches the selector.

## Commands In This Lesson

```bash
kubectl get pods -o wide
minikube ip
```

## Important Output

```text
NAME                                 READY   STATUS    RESTARTS   AGE     IP          NODE             NOMINATED NODE   READINESS GATES
client-deployment-576bc76988-mp7wn   1/1     Running   0          6m50s   10.1.0.15   docker-desktop   <none>           <none>
```

The important part is that the pod has its own IP address.

## Stable Path Versus Unstable Path

- unstable idea:
  browser -> specific pod IP like `172.0.0.1:3000`
- stable idea:
  browser -> node IP + service port like `<node-ip>:31515`

The service then forwards traffic to the matching pod on port `3000`.

## Why The Service Helps

- pods come and go
- pod IPs can change
- the service keeps watching pods that match its selector
- the selector in this lesson is `component: web`
- traffic keeps flowing without you changing the browser address every time

## Runtime Caveat

The lesson speaks in `minikube` terms, but the captured pod output shows `docker-desktop` as the node. So the big idea is stable, even if the exact local access method depends on your runtime.

## Retrieval Cues

- "services hide changing pod IPs"
- "pod IPs are internal and unstable"
- "`kubectl get pods -o wide` reveals pod IPs"
- "`component: web` is how the service finds the right pods"
