
Now I have the exact picture. Look at [postgres-deployment.yaml:29-33](vscode-webview://09g99m1u2tdb6lu1n0trdn2ldr73f5e1u6lhn34k0jlcpjpbrpua/k8s/postgres-deployment.yaml#L29-L33):

```yaml
env:
  - name: POSTGRES_PASSWORD
    valueFrom:
      secretKeyRef:
        name: pgpassword
        key: PGPASSWORD
```

Your postgres pod expects a **Kubernetes Secret** named `pgpassword` with a key `PGPASSWORD` to exist in the cluster _before_ it starts. If the secret isn't there, the pod fails with `CreateContainerConfigError`.

So the real question is: **how does that Secret object get into the cluster?** You've got three common answers, in increasing order of sophistication.

## Option 1: Create it once, manually, via Cloud Shell (the course's answer)

Open GCP Cloud Shell (or your local terminal after `gcloud container clusters get-credentials ...`) and run:

```bash
kubectl create secret generic pgpassword \
  --from-literal PGPASSWORD='your-real-password-here'
```

That's it. The Secret lives in etcd inside your cluster from that moment on. Your deploy workflow never sees the password. You never commit it to git. `kubectl apply -f k8s` just finds the secret already sitting there.

**Pros:** Dead simple. Password never crosses through GitHub. **Cons:** Manual step → not reproducible if your cluster is rebuilt. If someone runs `kubectl delete secret pgpassword`, your next deploy breaks with no obvious audit trail.

**Verdict:** This is what Stephen Grider's course teaches and it's the right place to start. Do this today.

## Option 2: Store the password as a GitHub Actions secret, create the k8s Secret in the workflow

You put the password into **GitHub repo settings → Secrets and variables → Actions** as `PG_PASSWORD`. Then you add a step to [deploy.yaml](vscode-webview://09g99m1u2tdb6lu1n0trdn2ldr73f5e1u6lhn34k0jlcpjpbrpua/.github/workflows/deploy.yaml), before `kubectl apply`:

```yaml
- name: Ensure database secret exists
  env:
    PG_PASSWORD: ${{ secrets.PG_PASSWORD }}
  run: |
    kubectl create secret generic pgpassword \
      --from-literal PGPASSWORD="$PG_PASSWORD" \
      --dry-run=client -o yaml | kubectl apply -f -
```

The `--dry-run=client -o yaml | kubectl apply -f -` trick is the idiomatic way to do "create or update" — `kubectl create secret` alone would fail on the second run because the secret already exists.

**Pros:** Reproducible. If the cluster gets rebuilt, the next deploy re-creates the secret. **Cons:** The password now lives in GitHub's secret store. Anyone with write access to the workflow file can exfiltrate it by adding a malicious `echo` step. You're trusting GitHub's secret handling.

**Verdict:** Fine upgrade once you're comfortable with Option 1. Good for side projects.

## Option 3: GCP Secret Manager + External Secrets Operator (production-grade)

You store the password in **GCP Secret Manager**. You install the **External Secrets Operator** in your cluster. You commit a harmless `ExternalSecret` YAML that says _"sync the `pgpassword` k8s Secret from this GCP Secret Manager entry."_ The operator keeps them in sync.

**Pros:** Secret never touches GitHub. Rotation is one-click in GCP. Audit logs. Same pattern scales to 100 secrets. **Cons:** One more component to install and learn. Overkill for a learning project.

**Verdict:** This is what you'd do at Deep Presence for real production workloads. Not today.

## What you should actually do

For where you are in the course:

1. **Right now, use Option 1.** Open Cloud Shell, run the `kubectl create secret generic pgpassword ...` command once. Done. Your pod starts, you move on learning.
2. **When you come back in a week or two**, try upgrading to Option 2 in your workflow. It'll reinforce the "kubectl is just a client talking to the API server" concept from earlier — because this step is literally the runner creating a Secret object in the cluster the same way it creates Deployments.
3. **When you deploy something real for Deep Presence**, go straight to Option 3. Don't let production secrets live in GitHub.

## One critical warning

**Never** put the password directly in a YAML file and commit it to git. Not even base64-encoded (which is what k8s Secret manifests use) — base64 is encoding, not encryption. Anyone with repo access decodes it in one command. This is the #1 mistake people make when they first discover Kubernetes Secrets.

![[Pasted image 20260422212525.png]]
