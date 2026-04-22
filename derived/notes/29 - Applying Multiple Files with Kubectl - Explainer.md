# Applying Multiple Files with Kubectl Explainer

## Source

- Transcript: `raw/transcripts/29 - Applying Multiple Files with Kubectl.md`
- Wiki source page: [[29 - Applying Multiple Files with Kubectl]]

## What This Lesson Is Doing

The last two lessons created two config files:

- a frontend deployment
- a frontend `ClusterIP` service

This lesson stops before moving on and checks whether those files actually work in Kubernetes.

That makes this a workflow lesson more than a concept lesson.

It shows how to:

- clean up the old resources
- apply multiple files together
- detect a YAML mistake
- fix it
- apply everything again
- verify the new resources

## Step 1: Remove The Old Frontend Resources

The lesson starts by deleting the older learning-version resources:

```bash
kubectl get deployments
kubectl delete deployment client-deployment
kubectl get services
kubectl delete service client-node-port
```

This is important because the course is replacing the old setup with the newer frontend deployment plus internal service.

So the operator is not just adding new things on top.

They are cleaning out the old version first.

## Step 2: Apply A Whole Directory

The main workflow shortcut is:

```bash
kubectl apply -f k8s
```

This means:

- do not point at one file
- point at the whole directory
- let `kubectl` apply every manifest inside it

That becomes useful once the app has many config files.

The transcript says the project will end up with around 11 config files, so applying a directory is simpler than listing each file separately every time.

## Step 3: Understand The Partial Success

The first apply is very instructive because it does not fail in a clean all-or-nothing way.

It says:

```text
service/client-cluster-ip-service created
Error from server (BadRequest): error when creating "k8s/client-deployment.yaml": Deployment in version "v1" cannot be handled as a Deployment: json: cannot unmarshal string into Go struct field Container.spec.template.spec.containers.ports of type v1.ContainerPort
```

This teaches an important operational lesson:

one manifest can succeed while another one fails.

So after a multi-file apply, you should not assume everything either fully worked or fully failed.

You have to read the output carefully.

## Step 4: Fix The YAML And Re-Apply

The instructor then notices a typo in the deployment file and fixes it.

The spoken explanation says the issue was `label` versus `labels`, while the server error shown on screen points more toward a malformed `ports` structure.

The safe takeaway is:

there was a YAML problem in the deployment manifest, it was corrected, and then the same directory-wide apply was run again.

After that, the output becomes:

```text
service/client-cluster-ip-service unchanged
deployment.apps/client-deployment created
```

That is a very normal Kubernetes pattern:

- a resource that already exists may show as `unchanged`
- a missing resource may show as `created`

## Step 5: Verify The New State

The lesson then checks the results with:

```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

The deployment output shows:

```text
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
client-deployment   3/3     3            3           14s
```

That means the new frontend deployment is now running with three ready pods, which matches the deployment config introduced earlier.

## Why Browser Testing Still Does Not Work Yet

The lesson makes one important point before any of the commands:

even if these resources apply correctly, you still cannot easily test the frontend in the browser yet.

Why:

- the frontend service is now `ClusterIP`
- `ClusterIP` is internal-only
- there is still no outside-facing access path

So success here means:

- the objects exist
- the pods run
- the service exists

It does not yet mean:

- a browser can reach the app

That step still depends on later networking config, especially `Ingress`.

## Why This Lesson Matters

This lesson is important because it teaches the everyday operator habit:

write configs, apply them together, read the output carefully, fix what broke, then verify the cluster state.

It is also the first moment where the course shows that Kubernetes config work is not just about writing YAML.

It is also about:

- applying groups of manifests
- handling partial failure
- checking the actual cluster state after each apply

## Important Caveats

- The transcript's spoken diagnosis of the deployment typo does not perfectly match the server error text, so the exact original typo is not fully reliable.
- The final `kubectl get services` output appears truncated in the transcript, so the lesson proves service creation but does not preserve the full final table.
- The app still cannot be browser-tested at this stage because there is no outside-facing route yet.

## Best Reuse

Use this note when you want to remember:

- how to apply every manifest in a folder with `kubectl`
- why a multi-file apply can partially succeed
- why you should always read the full apply output
- how to verify the cluster after fixing and re-applying manifests
