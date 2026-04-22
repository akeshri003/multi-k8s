# Wiki Index

Read this file first for most queries and maintenance work.

## Control

- [[AGENTS]] - Operating rules for how the agent should maintain this repository.
- [[wiki/overview]] - Current scope, structure, and near-term state of the wiki.
- [[wiki/log]] - Append-only operational history.

## Sources

- [[2026-04-17-llm-wiki-pattern]] - Foundational source describing the persistent LLM-maintained wiki pattern.
- [[The Whys and What's of Kubernetes -]] - Introductory Kubernetes transcript covering clusters, nodes, masters, and the orchestration use case.
- [[2 - Kubernetes in Development and Production]] - Kubernetes note on local development with minikube versus production use with managed services and kubectl.
- [[3 - Mapping Existing Knowledge]] - Docker Compose to Kubernetes mapping note covering prebuilt images, object-based config, and manual networking.
- [[4 - Adding Config files]] - First concrete Kubernetes manifests: a client Pod and a NodePort Service.
- [[5 - Object Types and API Versions]] - Explanation of `kind`, `apiVersion`, object types, and how manifests become cluster objects.
- [[6 - Running Container in Pods]] - Pod deep dive covering the smallest deployable unit and when multiple containers belong together.
- [[7 - Service Config Files in Depth]] - Service and NodePort deep dive covering selectors, kube-proxy, and the three service port fields.
- [[8 - Connecting to Running Containers]] - Operational walkthrough for applying manifests, checking pod/service status, and reaching a NodePort-backed workload.
- [[9 - Entire Deployment Flow]] - Behind-the-scenes explanation of how the master tracks desired state, assigns work across nodes, and restores missing workload copies.
- [[10 - Imperative vs Declarative Deployements]] - Comparison of imperative and declarative deployment styles, arguing that Kubernetes should usually be used declaratively through config updates.
- [[11 - Updating Existing Objects]] - Practical declarative update example showing how re-applying the original config changes an existing pod instead of creating a new one when identity fields stay stable.
- [[12 - Declarative Updates in Action]] - Concrete verification walkthrough showing that `kubectl apply` updates the existing pod and that `kubectl describe` proves the image and lifecycle changes.
- [[13 - Limitations in Config Updates]] - Shows that declarative pod updates have strict limits by rejecting a `containerPort` change even when the manifest is re-applied correctly.
- [[14 - Running Containers with Deployments]] - Introduces deployments as the main object for managing application pods once direct pod updates become too limited.
- [[15 - Deployment Configuration files.]] - First concrete deployment manifest showing `apps/v1`, `replicas`, `selector.matchLabels`, and the pod `template` block.
- [[16 - Walkthrough Deployment Config]] - Explains why `replicas`, `selector.matchLabels`, and `template.metadata.labels` exist and how the deployment finds the pods it manages.
- [[17 - Applying a Deployment]] - First real deployment rollout showing pod deletion, deployment creation, generated pod names, and how to read `kubectl get deployments`.
- [[18 - Why use Services?]] - Explains why services still matter after deployments: pod IPs are internal and unstable, so the service remains the stable selector-based access point.
- [[19 - Scaling and Chaning Deployments]] - Shows how deployment template changes recreate pods, how `replicas` scales pod count, and how rollout status briefly shows both old and new pods at once.
- [[20 - Updating Deployment Images]] - Resets the deployment back to a single `multi-client` pod on port `3000` and stages the next image-version update problem.
- [[21 - Rebuilding the Client Image]] - Changes the React client text, rebuilds the `multi-client` image, and pushes the updated image to Docker Hub.
- [[22 - Triggering Deployment Updates]] - Explains why unchanged manifests do not refresh image contents and compares three workaround families for forcing deployments onto newer image versions.
- [[23 - Imperatively Updating a Deployment's Image]] - Gives the concrete `kubectl set image` workflow for moving a deployment onto a newly tagged image and verifying the rollout through pod age and browser output.
- [[24 - The Path to Production]] - Introduces the full multi-service Kubernetes architecture for the Fibonacci app and lays out the local-to-cloud path through config files, minikube testing, CI/CD, and cloud deployment.
- [[25 - A Quick Checkpoint]] - Verifies the old Docker Compose app still works locally before starting the Kubernetes migration, including the Docker-context check and `localhost:3050` sanity test.
- [[26 - Recreating the Deployment]] - Starts the real manifest migration by removing old deployment files, creating the `k8s/` folder, and rebuilding the frontend as a three-replica Kubernetes deployment.
- [[27 - NodePort vs ClusterIP Services]] - Explains the networking shift from outside-facing `NodePort` services to internal-only `ClusterIP` services that sit behind `Ingress`.
- [[28 - The ClusterIP Config]] - Creates the actual frontend `ClusterIP` service manifest and shows that internal-only services keep `port` and `targetPort` but drop `nodePort`.
- [[29 - Applying Multiple Files with Kubectl]] - Shows how to clean up the old frontend resources, apply the whole `k8s/` directory at once, fix a deployment YAML error, and verify the new deployment plus `ClusterIP` service.
- [[30 - Express API deployment config]] - Creates the backend `server-deployment` manifest for `multi-server`, reusing the deployment pattern with `component: server`, port `5000`, and deferred Redis/Postgres env vars.
- [[31 - ClusterIP for Express API]] - Creates the backend `server-cluster-ip-service` manifest, targeting `component: server` and routing internal traffic through port `5000`.
- [[32 - Combining Config Into Single Files]] - Explains how multiple Kubernetes objects can live in one YAML file separated by `---`, while still recommending one file per object for clarity.
- [[33 - The Worker Deployment]] - Creates the `worker-deployment`, explains why the worker starts at one replica, and clarifies why it needs no service or exposed port.
- [[34 - Reapplying Batch of Config Files]] - Reapplies the full `k8s/` directory, shows the new server/worker objects being created, and uses `kubectl logs` to reveal that running pods can still be functionally miswired.
- [[35 - Volumes vs Persistent Volumes]] - Despite its title, creates the Redis deployment and `redis-cluster-ip-service`, using port `6379` and a single Redis replica.
- [[36 - Last set of config]] - Creates the Postgres deployment and `postgres-cluster-ip-service`, closing the repetitive deployment/service phase before environment variables and persistence.
- [[37 - The need for Volumes with Database]] - Explains why Postgres data cannot safely live only in the container filesystem and why persistence matters before PVCs are introduced.
- [[38 -Kubernetes Volumes]] - Clarifies that a Kubernetes `Volume` is a specific pod-level storage object and explains why it still is not enough for durable Postgres data.
- [[39 - Volume vs Persistent Volume]] - Compares pod-level `Volume` storage with external `PersistentVolume` storage and makes the lifecycle boundary explicit: pod-owned storage dies with the pod, but a persistent volume does not.
- [[40 - Persistent Volume vs Persistent Volume Claim]] - Explains that a `PersistentVolumeClaim` is the storage request layer while a `PersistentVolume` is the actual storage resource, and introduces static versus dynamic provisioning.
- [[41 - Claim Config files]] - Creates the first real `PersistentVolumeClaim` manifest for the Postgres setup, requesting `ReadWriteOnce` access and `2Gi` of storage.
- [[42 - Persistent Volume Access Modes]] - Explains what `ReadWriteOnce`, `ReadOnlyMany`, `ReadWriteMany`, and `storage: 2Gi` mean inside the PVC spec.
- [[43 - Where does Kubernetes Allocate Persistent Volumes]] - Shows how Kubernetes decides where persistent storage comes from, using the default `StorageClass` locally and broader provisioner choices in the cloud.
- [[44 - Designating a PVC in a POD Template]] - Wires the claim into `postgres-deployment` through `volumes` and `volumeMounts`, including `mountPath` and the Postgres-specific `subPath`.
- [[45 - Applying a PVC]] - Applies the PVC-backed Postgres config and uses `kubectl get pods`, `kubectl get pv`, and `kubectl get pvc` to verify that the persistent volume exists and is bound to the claim.
- [[46 -Defining Environment Variables]] - Starts the runtime wiring phase by separating ordinary constant environment variables from connection-oriented values used to reach Redis and Postgres.
- [[47 - Adding Environment Variables to Config]] - Adds the first real `env` blocks to the worker and server deployments, wiring Redis and most Postgres connection values through service names and default ports.
- [[48 - Creating an Encoded Secret]] - Introduces Kubernetes `Secret` for `PGPASSWORD` and shows the imperative `kubectl create secret generic ... --from-literal ...` workflow.
- [[49 - Postgres Environment Variable Fix]] - Wires the `pgpassword` secret into the server and Postgres deployments with `valueFrom.secretKeyRef`, then hits the next real YAML problem: `cannot convert int64 to string`.
- [[50 - Environment Variables as Strings]] - Fixes the env typing problem by quoting numeric port values as strings, which allows `kubectl apply -f k8s` to succeed again.
- [[51 - Load Balancer Services]] - Revisits the `LoadBalancer` service type, explains why it is considered older than ingress for this routing problem, and shows why a single load-balancer service is not a good fit for this app.
- [[52. - Quick Note on Ingress]] - Introduces ingress at a high level and gives the critical project distinction between `kubernetes/ingress-nginx` and `nginxinc/kubernetes-ingress`.
- [[53 - Ingress setup notes]] - Warns that ingress-nginx setup is environment-specific and that the course will cover local setup and later Google Cloud setup.
- [[54 - Behind the Scenes of Ingress]] - Explains ingress through the controller mental model, showing how ingress config and ingress controllers fit the same desired-state pattern as deployments.
- [[55 - More Behind the Scenes of Ingress]] - Maps ingress-nginx onto the real app architecture on Google Cloud, including the cloud load balancer, in-cluster `LoadBalancer` service, nginx controller pod, and default backend.
- [[56 - Ingress Nginx installation and setup]] - Gives the Docker Desktop local setup pointer for ingress-nginx by sending the reader to the official quick-start docs and the "If you don't Have Helm" install section.
- [[57 - Creating Ingress Configuration]] - Creates the first ingress manifest, routing `/` to `client-cluster-ip-service`, routing `/api/` to `server-cluster-ip-service`, and rewriting `/api` away before the request reaches the server.
- [[59 - The Deployment Process]] - Turns the production path into a concrete checklist: repo, CI, Google Cloud project, billing, deployment scripts, and GKE Autopilot cluster creation.
- [[60 - CI Setup]] - Explains the course's Travis-based build-test-push-deploy pipeline and why deployment image updates still need an explicit CI/CD step after manifest apply.
- [[61 - Installing google cloud sdk]] - Shows the old Travis bootstrap for Google Cloud SDK and `kubectl`, then maps it to the modern GitHub Actions auth and GKE-credentials approach.
- [[62 - Service Account Steps for GCP]] - Shows the old GCP service-account JSON key creation flow for Travis CI and explains why modern GitHub Actions should prefer federation-based auth instead.
- [[63 - Unique Deployment Images]] - Drafts the first real `deploy.sh` flow for building, pushing, applying manifests, and setting deployment images, then shows why `latest` alone is still not enough.
- [[64 - Unique Tags for Built Image]] - Introduces Git commit SHAs as the unique image tags that make deployment updates deterministic while keeping `latest` for fresh applies.
- [[65 - Updating the Deployment Script]] - Applies the SHA-tag strategy to `.travis.yml` and `deploy.sh`, including dual-tag builds, dual pushes, and SHA-specific deployment updates.
- [[66 - Configuring the gcloud CLI on the cloud console]] - Shows how to target the live GKE cluster from Cloud Shell before creating the missing remote secret.
- [[67 - Helm and its setup]] - Records the current Helm-based ingress-nginx install step for GKE and the reason the ingress controller must be installed separately from the app manifests.
- [[68 -]] - Placeholder source file that contains only a screenshot reference and no prose transcript, so it remains incomplete even though the asset is now present locally.
- [[68 - The result of ingress-nginx]] - Shows what appears in GKE after ingress-nginx is installed: the controller, default backend, `LoadBalancer` service, external IP, and the Google Cloud load balancer in front of the cluster.
- [[69 - HTTPS SETUP OVERVIEW]] - High-level explanation of the HTTPS flow using Let's Encrypt, including the domain-ownership challenge and certificate-renewal idea.
- [[70 - Cert Manager Install]] - Installs cert-manager through a modern Helm flow and preserves the updated commands instead of the older video path.
- [[71 - How to wire up Cert Manager]] - Explains the two-object cert-manager model: issuer for where/how to ask, certificate for what cert to obtain and where to store it.
- [[72 - Issuer Config File]] - Preserves the modern `ClusterIssuer` manifest for Let's Encrypt production, including the nginx HTTP-01 solver.
- [[73 - Certificate Config File]] - Preserves the modern `Certificate` manifest shape, including `secretName`, `issuerRef`, and the covered DNS names.
- [[74 - Deploying Changes]] - Clarifies the deploy order for HTTPS: create issuer and certificate first, let cert-manager create the secret, then update ingress to use it.
- [[75 - Ingress for Config]] - Adds the HTTPS-specific ingress wiring: the cert-manager issuer annotation, SSL redirect, TLS hosts, and TLS secret reference.
- [[76 - skaffold overview]] - Introduces Skaffold as the missing local-development loop for Kubernetes, contrasting rebuild/redeploy with direct file sync.
- [[77 - Skaffold Config]] - Modernizes the Skaffold setup and separates shared, dev-only, and prod-only Kubernetes manifests.
- [[78 - Live sync changes]] - Demonstrates `skaffold dev`, direct file sync into running containers, and rebuild fallback when sync rules do not match.
- [[2026-04-22-mental-model-of-github-actions-gke-deployment]] - Clarifies that the deployment workflow runs on a temporary GitHub-hosted runner and that `kubectl` only sends API requests into GKE.
- [[2026-04-22-managing-secrets-for-cloud-deployment]] - Compares three ways to get secrets into the live cluster: manual Cloud Shell creation, GitHub Actions-managed secret creation, and Secret Manager plus an operator.

## Topics

- [[llm-wiki]] - Core topic covering the wiki-as-memory-layer pattern and its operating implications.
- [[kubernetes]] - Topic page for what Kubernetes is, when it becomes useful, and the cluster mental model.

## Entities

- None yet.

## Analysis

- [[kubernetes-object-identity]] - Durable answer explaining that `Pod` is one object kind, while practical object identity is usually `kind + namespace + name` and permanent identity is `metadata.uid`.
- [[kubernetes-smallest-deployable-unit]] - Durable answer explaining that `Pod`, not just any object, is the smallest deployable workload unit in Kubernetes.
- [[kubernetes-modern-ingress-format]] - Durable migration note explaining why the course's old ingress YAML fails on a modern cluster and what the stable `networking.k8s.io/v1` ingress format looks like.
- [[github-actions-gke-deployment-flow]] - Durable modernization note translating the course's Travis-based GKE deployment pipeline into a current GitHub Actions workflow using official Google and Docker actions.
- [[multi-k8s-google-cloud-ui-setup-for-github-actions]] - Project-specific step-by-step note for wiring the `multi-k8s` GitHub repo to Google Cloud through Workload Identity Federation and the Google Cloud UI.
- [[multi-k8s-github-actions-files-and-flow]] - Explains exactly where `ci.yaml` and `deploy.yaml` belong, why they should be separate, and what each part of the current `multi-k8s` workflows means.
- [[github-actions-yaml-patterns-for-ci-cd]] - First-principles guide to how CI and CD workflow YAML files are structured, why they are written that way, and how to map that pattern onto `multi-k8s`.
- [[kubernetes-cloud-deployment-secret-management]] - Durable recommendation ladder for managing secrets in cloud Kubernetes deploys, from manual cluster secrets to Secret Manager plus an operator.
- [[kubernetes-command-cheat-sheet]] - Cumulative shell-command reference for the Kubernetes material learned so far, grouped by local workflow, deployments, storage, secrets, GKE, and ingress.
- [[kubernetes-https-cert-manager-flow]] - Consolidated explanation of the HTTPS setup path across ingress-nginx, cert-manager, `ClusterIssuer`, `Certificate`, the TLS secret, and the final ingress TLS wiring.
- [[skaffold-local-kubernetes-development]] - Consolidated explanation of the new local Kubernetes development loop using Skaffold, dev/prod manifest separation, and sync-versus-rebuild behavior.

## Maps

- None yet.

## Questions

- [[modern-local-kubernetes-development-setup]] - Open question tracking the gap between older minikube setup advice and current local Kubernetes tooling.

## Schemas

- [[schemas/checklists/ingest-checklist]] - Step-by-step process for standard source ingest.
- [[schemas/checklists/video-ingest-checklist]] - Specialized checklist for video sources and transcript quality.
- [[schemas/templates/source-page-template]] - Template for `wiki/sources/` pages.
- [[schemas/templates/topic-page-template]] - Template for `wiki/topics/` pages.
- [[schemas/templates/entity-page-template]] - Template for `wiki/entities/` pages.
- [[schemas/templates/analysis-page-template]] - Template for `wiki/analysis/` pages.
- [[schemas/templates/question-page-template]] - Template for `wiki/questions/` pages.
