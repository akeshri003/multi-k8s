# Applying Multiple Files with Kubectl Cheat Sheet

## Source

- Transcript: `raw/transcripts/29 - Applying Multiple Files with Kubectl.md`
- Wiki source page: [[29 - Applying Multiple Files with Kubectl]]

## Key Idea

Instead of applying each manifest one by one, you can apply the whole `k8s/` folder at once.

## Cleanup First

The lesson first removes the older frontend resources:

```bash
kubectl get deployments
kubectl delete deployment client-deployment
kubectl get services
kubectl delete service client-node-port
```

Why:

- the old frontend deployment was from the earlier learning setup
- the old service was a `NodePort`
- the new setup is moving to a deployment plus `ClusterIP` service

## The Main Shortcut

```bash
kubectl apply -f k8s
```

Meaning:

- `kubectl` looks inside the `k8s/` directory
- it finds each config file
- it applies all of them

## Important Result

The first apply does not fully work.

It creates the service, but the deployment fails because of a typo in the deployment YAML.

```text
service/client-cluster-ip-service created
Error from server (BadRequest): error when creating "k8s/client-deployment.yaml": Deployment in version "v1" cannot be handled as a Deployment: json: cannot unmarshal string into Go struct field Container.spec.template.spec.containers.ports of type v1.ContainerPort
```

## After Fixing The Typo

Running the same command again works:

```text
service/client-cluster-ip-service unchanged
deployment.apps/client-deployment created
```

## How To Verify

```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

The transcript shows:

```text
client-deployment   3/3   3   3   14s
```

## Important Limitation

- the pods and service can be created successfully
- but you still cannot test the app in the browser yet
- the service is `ClusterIP`, so it does not expose outside traffic

## Retrieval Cues

- "`kubectl apply -f k8s` applies a whole folder"
- "one bad file can fail while another file still gets created"
- "fix the YAML, then re-apply the directory"
- "`ClusterIP` means no browser test yet"
