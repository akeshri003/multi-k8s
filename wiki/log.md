# Wiki Log

Append-only operational history for the knowledge base.

## [2026-04-17] bootstrap | initial scaffold

- Trigger: repository bootstrap from the initial LLM wiki idea file.
- Created or updated: [[AGENTS]], [[wiki/index]], [[wiki/log]], [[wiki/overview]], [[schemas/checklists/ingest-checklist]], [[schemas/checklists/video-ingest-checklist]].
- Key decisions: the canonical control surface now lives under `wiki/` and `schemas/`; `raw/` remains immutable; `derived/` is the only place for agent-created prep artifacts.
- Follow-up: keep future work aligned to the new taxonomy of sources, topics, entities, analysis, maps, and questions.

## [2026-04-17] ingest | llm wiki idea file

- Trigger: ingest of the foundational idea file stored at `raw/library/2026-04-17-llm-wiki-idea.md`.
- Created or updated: [[2026-04-17-llm-wiki-pattern]], [[llm-wiki]], [[wiki/index]], [[wiki/overview]].
- Key synthesis: the repository should treat the wiki as the durable memory layer, not transient chat answers or repeated query-time retrieval.
- Follow-up: add the next real source and test the standard ingest workflow against a non-bootstrap example.

## [2026-04-17] restructure | align scaffold with four-layer workflow

- Trigger: request to make the repository workflow match the four-layer model with `raw/`, `derived/`, `wiki/`, and `schemas/`.
- Created or updated: [[AGENTS]], [[wiki/index]], [[wiki/log]], [[wiki/overview]], [[llm-wiki]], [[2026-04-17-llm-wiki-pattern]], plus new `derived/`, `schemas/`, `wiki/topics/`, `wiki/analysis/`, `wiki/maps/`, and `wiki/questions/` directories.
- Key decisions: new canonical files use `wiki/index.md`, `wiki/log.md`, and `wiki/overview.md`; root `index.md` and `log.md` are now compatibility pointers; the old concept and operations notes are superseded by topic and schema files.
- Follow-up: use the next ingest to validate whether a question page or entity page is warranted.

## [2026-04-17] ingest | the whys and what's of kubernetes

- Trigger: ingest request for the newly added transcript and screenshots in `raw/transcripts/` and `raw/assets/`.
- Created or updated: [[The Whys and What's of Kubernetes -]], [[kubernetes]], [[wiki/index]], and two derived notes in `derived/notes/` for cheat-sheet and explanatory reuse.
- Key synthesis: this source frames Kubernetes as valuable when containerized applications require heterogeneous workloads and uneven scaling across multiple machines, with a control layer coordinating nodes behind a load balancer.
- Follow-up: ingest a more detailed Kubernetes source to map `master` terminology onto modern control-plane, pod, service, and deployment concepts.

## [2026-04-17] ingest | kubernetes in development and production

- Trigger: ingest request for the newly added note at `raw/transcripts/2 - Kubernetes in Development and Production.md`.
- Created or updated: [[2 - Kubernetes in Development and Production]], [[kubernetes]], [[modern-local-kubernetes-development-setup]], [[wiki/index]], and [[wiki/log]].
- Key synthesis: this source adds the local-versus-production operating split, presenting `minikube` as the local cluster tool, `kubectl` as the cross-environment control interface, and managed services like EKS and GKE as common production options.
- Follow-up: ingest a newer Kubernetes setup source to resolve whether the `VirtualBox` and `minikube` flow is still the right default for modern local development.

## [2026-04-17] ingest | mapping existing knowledge

- Trigger: ingest request for the newly added transcript at `raw/transcripts/3 - Mapping Existing Knowledge.md` and its referenced screenshots.
- Created or updated: [[3 - Mapping Existing Knowledge]], [[kubernetes]], [[wiki/index]], and [[wiki/log]].
- Key synthesis: this source maps Docker Compose concepts into Kubernetes, emphasizing that Kubernetes expects prebuilt images, uses multiple config files to create objects rather than a single container list, and requires more explicit networking.
- Follow-up: ingest the next source that names the exact Kubernetes object and networking primitives used in the early `multi-client` deployment example.

## [2026-04-17] restructure | canonicalize mapping-existing-knowledge screenshots

- Trigger: the three screenshots referenced by [[3 - Mapping Existing Knowledge]] were moved into `raw/assets/`.
- Created or updated: [[3 - Mapping Existing Knowledge]] and [[wiki/log]].
- Key decisions: source evidence now points only at canonical `raw/` paths; the prior caveat about non-canonical screenshot storage is no longer needed.
- Follow-up: keep future screenshot assets inside `raw/assets/` before ingest when possible.

## [2026-04-17] ingest | adding config files and object types/api versions

- Trigger: ingest request for the newly added transcripts `raw/transcripts/4 - Adding Config files.md` and `raw/transcripts/5 - Object Types and API Versions.md`, plus the related screenshots in `raw/assets/`.
- Created or updated: [[4 - Adding Config files]], [[5 - Object Types and API Versions]], [[kubernetes]], [[wiki/index]], and four derived notes in `derived/notes/` with embedded assets and fenced YAML for easier review.
- Key synthesis: these sources move the wiki from high-level Kubernetes framing into the first concrete manifest model: `Pod` and `Service` as object kinds, `NodePort` as the first networking pattern, and `kind` plus `apiVersion` as the keys that define what a manifest creates.
- Follow-up: ingest the next source that explains how these introductory `Pod` and `Service` examples evolve into more typical deployment patterns and clarifies the object model after the first local demo.

## [2026-04-17] ingest | running container in pods and service config files in depth

- Trigger: ingest request for the newly added transcripts `raw/transcripts/6 - Running Container in Pods.md` and `raw/transcripts/7 - Service Config Files in Depth.md`, plus the related screenshots.
- Created or updated: [[6 - Running Container in Pods]], [[7 - Service Config Files in Depth]], [[kubernetes]], [[wiki/index]], and four derived notes in `derived/notes/` that embed visuals and keep the manifest snippets inline for easier reading.
- Key synthesis: these sources explain why Kubernetes deploys pods rather than naked containers, when multiple containers should share a pod, how `Service` and `NodePort` work, how selectors match labels, and how `port`, `targetPort`, and `nodePort` fit together.
- Follow-up: canonicalize the newer root-level screenshots into `raw/assets/` and ingest the next networking source that moves beyond the development-only `NodePort` pattern.

## [2026-04-17] restructure | canonicalize pod and service deep-dive screenshots

- Trigger: the screenshots used by [[6 - Running Container in Pods]] and [[7 - Service Config Files in Depth]] were added to `raw/assets/`.
- Created or updated: [[6 - Running Container in Pods]], [[7 - Service Config Files in Depth]], the corresponding derived notes, and [[wiki/log]].
- Key decisions: pod and service deep-dive notes now embed canonical `raw/assets/` paths instead of relying on root-level screenshots.
- Follow-up: keep future screenshot references canonicalized before or immediately after ingest.

## [2026-04-17] restructure | embed visuals directly in source pages

- Trigger: request to show screenshots directly inside `wiki/sources/` pages instead of only listing asset paths in metadata.
- Created or updated: the screenshot-bearing pages under `wiki/sources/` plus [[wiki/log]].
- Key decisions: source notes now use direct Obsidian embeds in their metadata sections so readers can browse the source pages without opening assets separately.
- Follow-up: keep future `wiki/sources/` pages using direct embeds whenever visuals materially aid comprehension.

## [2026-04-17] restructure | rename derived notes to match transcript titles

- Trigger: request to name derived notes after the original files in `raw/transcripts/`.
- Created or updated: the transcript-derived note files under `derived/notes/`, the corresponding source-page references, and [[wiki/log]].
- Key decisions: derived notes now preserve the source transcript title in the filename, with ` - Cheat Sheet` and ` - Explainer` suffixes for the two note types.
- Follow-up: use transcript-title-based note names for future derived notes from transcript sources.

## [2026-04-17] restructure | rename source pages to match transcript titles

- Trigger: request to use the same transcript-title naming pattern for `wiki/sources/`.
- Created or updated: the transcript-derived source files under `wiki/sources/`, the corresponding links across the vault, and [[wiki/log]].
- Key decisions: transcript-derived source pages now mirror the filenames in `raw/transcripts/` so source, transcript, and derived-note naming stay aligned.
- Follow-up: use transcript-title-based filenames for future transcript-derived source pages.

## [2026-04-17] ingest | connecting to running containers

- Trigger: ingest request for the newly added transcript `raw/transcripts/8 - Connecting to Running Containers.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[8 - Connecting to Running Containers]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with embedded visuals and inline command/output blocks.
- Key synthesis: this source turns the earlier pod-and-service setup into the first complete operator loop by showing how to apply manifests, verify pod and service state with `kubectl get`, and access the workload through `NodePort`, while preserving the runtime-specific difference between `minikube` VM access and Docker Desktop localhost access.
- Follow-up: ingest the next source that moves from basic `kubectl get` inspection into the next layer of debugging, deployment management, or production-oriented networking.

## [2026-04-17] ingest | entire deployment flow

- Trigger: ingest request for the newly added transcript `raw/transcripts/9 - Entire Deployment Flow.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[9 - Entire Deployment Flow]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with embedded visuals and inline command blocks.
- Key synthesis: this source explains the control-loop model behind Kubernetes by showing that configuration goes to the master, the simplified `Kube API Server` tracks desired state, nodes pull images and run assigned workloads, and crashed containers get recreated so the requested replica count is restored.
- Follow-up: ingest the next source that names the higher-level Kubernetes object responsible for keeping replica counts stable and maps this simplified control-flow story to the fuller modern control-plane model.

## [2026-04-17] ingest | imperative vs declarative deployements

- Trigger: ingest request for the newly added transcript `raw/transcripts/10 - Imperative vs Declarative Deployements.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[10 - Imperative vs Declarative Deployements]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with embedded visuals and the transcript's key config/version examples inline.
- Key synthesis: this source makes the workflow preference explicit by contrasting imperative migration planning with declarative desired-state updates, and it argues that real Kubernetes work, especially in production, should center on editing configuration files and letting Kubernetes perform reconciliation.
- Follow-up: ingest the next source that shows the higher-level object model used for declarative replica management and clarifies when imperative `kubectl` commands are still acceptable for debugging.

## [2026-04-19] query | kubernetes object identity

- Trigger: question about whether "object" means `Pod`, how a unique object is defined, and what the signature of a pod object is.
- Created or updated: [[kubernetes-object-identity]], [[wiki/index]], and [[wiki/log]].
- Key synthesis: `object` is the general Kubernetes API record, `Pod` is one object kind, practical identity is usually `kind + metadata.name + metadata.namespace`, and permanent identity is `metadata.uid`.
- Follow-up: add a similar note if future questions need a compact explanation of how `Deployment`, `ReplicaSet`, and `Pod` identities relate to each other.

## [2026-04-19] ingest | updating existing objects

- Trigger: ingest request for the newly added transcript `raw/transcripts/11 - Updating Existing Objects.md` and its related screenshots.
- Created or updated: [[11 - Updating Existing Objects]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with embedded visuals and an inline manifest example that shows the image update described in the transcript.
- Key synthesis: this source turns the declarative workflow into a concrete update pattern by showing that you modify the original config file, keep the object's matching fields stable, and re-apply it so Kubernetes updates the existing object rather than creating a new one.
- Follow-up: if the missing third screenshot is later moved into `raw/assets/`, canonicalize the source and derived notes to remove the non-canonical asset caveat.

## [2026-04-19] query | kubernetes smallest deployable unit

- Trigger: question about whether the smallest deployable unit is any object or `Pod` specifically.
- Created or updated: [[kubernetes-smallest-deployable-unit]], [[wiki/index]], and [[wiki/log]].
- Key synthesis: `Pod` is the smallest deployable workload unit in Kubernetes; `object` is the broader category and includes many non-deployable config or control objects.
- Follow-up: add a compact note later if needed on how `Pod`, `ReplicaSet`, and `Deployment` relate as deployable versus managing objects.

## [2026-04-19] ingest | declarative updates in action

- Trigger: ingest request for the newly added transcript `raw/transcripts/12 - Declarative Updates in Action.md` and its related screenshots.
- Created or updated: [[12 - Declarative Updates in Action]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with embedded visuals and inline command/output blocks for the verification workflow.
- Key synthesis: this source demonstrates the declarative update loop end to end by changing the pod image, re-applying the manifest, confirming that only one `client-pod` still exists, and using `kubectl describe pod client-pod` plus lifecycle events to prove that the existing object was updated in place.
- Follow-up: if `Screenshot 2026-04-19 at 3.05.02 PM.png` is later moved into `raw/assets/`, canonicalize the source and derived notes to remove the non-canonical asset caveat.

## [2026-04-19] ingest | limitations in config updates

- Trigger: ingest request for the newly added transcript `raw/transcripts/13 - Limitations in Config Updates.md` and its related screenshot in `raw/assets/`.
- Created or updated: [[13 - Limitations in Config Updates]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the error output and rejected field change preserved inline.
- Key synthesis: this source tightens the declarative-update story by showing that direct pod updates are only allowed for a narrow set of fields, and that changing `containerPort` is rejected even though changing the image was allowed in the previous lesson.
- Follow-up: ingest the next source that explains the workaround for this limitation and introduces the higher-level object model that handles ongoing application changes more realistically.

## [2026-04-19] ingest | running containers with deployments

- Trigger: ingest request for the newly added transcript `raw/transcripts/14 - Running Containers with Deployments.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[14 - Running Containers with Deployments]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with embedded visuals and the pod-versus-deployment comparison preserved in simplified form.
- Key synthesis: this source introduces `Deployment` as the main higher-level object for real container management, explains that it maintains one or more identical pods through a pod template, and positions deployments as the practical answer to the limits of direct pod mutation.
- Follow-up: ingest the next source that shows the actual deployment YAML and the concrete steps for replacing the earlier bare `Pod` manifest with a deployment-based workflow.

## [2026-04-19] ingest | deployment configuration files.

- Trigger: ingest request for the newly added transcript `raw/transcripts/15 - Deployment Configuration files..md` and its related screenshot in `raw/assets/`.
- Created or updated: [[15 - Deployment Configuration files.]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the full deployment YAML preserved inline for study.
- Key synthesis: this source provides the first concrete deployment manifest in the course, showing that `Deployment` in `apps/v1` uses `replicas`, `selector.matchLabels`, and a pod `template` to manage pods instead of creating bare pods directly.
- Follow-up: ingest the next source that breaks this deployment file down line by line and clarifies how the selector, labels, and service wiring fit together.

## [2026-04-19] ingest | walkthrough deployment config

- Trigger: ingest request for the newly added transcript `raw/transcripts/16 - Walkthrough Deployment Config.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[16 - Walkthrough Deployment Config]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the deployment YAML and the selector/template explanation preserved in simplified form.
- Key synthesis: this source explains the internal logic of the first deployment manifest by showing that `template` defines the pods, `replicas` defines how many should exist, and `selector.matchLabels` is how the deployment finds and manages the pods it asked Kubernetes to create.
- Follow-up: ingest the next source that shows the deployment being applied and clarifies how the service and deployment selectors line up in practice.

## [2026-04-19] ingest | applying a deployment

- Trigger: ingest request for the newly added transcript `raw/transcripts/17 - Applying a  Deployment.md` and its related screenshot in `raw/assets/`.
- Created or updated: [[17 - Applying a Deployment]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the command sequence and deployment-status fields preserved inline.
- Key synthesis: this source shows the first practical deployment rollout by deleting the old direct pod with `kubectl delete -f`, applying `client-deployment.yaml`, observing the new deployment-managed pod, and introducing `kubectl get deployments` with the `desired`, `current`, `up-to-date`, and `available` columns.
- Follow-up: ingest the next source that verifies browser access or service routing against the new deployment-managed pod set.

## [2026-04-19] ingest | why use services

- Trigger: ingest request for the newly added transcript `raw/transcripts/18 - Why use Services?.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[18 - Why use Services?]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the screenshots and command blocks preserved inline.
- Key synthesis: this source explains why deployments do not eliminate the need for services by showing that pods have internal IPs that can change as they are recreated, while a service uses selectors such as `component: web` to keep traffic flowing to the current matching pod through a stable access point.
- Follow-up: ingest the next source that shows the lower-level mechanism behind service endpoint updates or the next networking abstraction beyond this NodePort-based development path.

## [2026-04-19] ingest | scaling and chaning deployments

- Trigger: ingest request for the newly added transcript `raw/transcripts/19 - Scaling and Chaning Deployments.md`.
- Created or updated: [[19 - Scaling and Chaning Deployments]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the command workflow, rollout numbers, and supporting manifest visuals preserved inline.
- Key synthesis: this source shows that changing a deployment template can recreate pods, increasing `replicas` scales the number of managed pods, and changing the image triggers a rollout where old and new pods briefly coexist, allowing `current` to temporarily exceed `desired`.
- Follow-up: ingest the next source that explains the rollout strategy or production caveats behind this temporary over-provisioning behavior.

## [2026-04-19] ingest | updating deployment images

- Trigger: ingest request for the newly added transcript `raw/transcripts/20 - Updating Deployment Images.md` and its related screenshot in `raw/assets/`.
- Created or updated: [[20 - Updating Deployment Images]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the reset workflow and planning diagram preserved inline.
- Key synthesis: this source resets the deployment back to a single browser-testable `multi-client` pod on port `3000` and frames the next operational problem: rebuild a newer image, push it to Docker Hub, and then somehow get the deployment to recreate its pods from that newer image.
- Follow-up: ingest the next source that actually rebuilds the image and the one after that which explains why unchanged manifests still do not refresh pods.

## [2026-04-19] ingest | rebuilding the client image

- Trigger: ingest request for the newly added transcript `raw/transcripts/21 - Rebuilding the Client Image.md`.
- Created or updated: [[21 - Rebuilding the Client Image]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the source edit and Docker build/push steps preserved inline.
- Key synthesis: this source creates a visibly newer `multi-client` image by changing the browser text to `Fib calculator version two`, rebuilding the Docker image, and pushing it to Docker Hub, while leaving the deployment-update problem unsolved for the next lesson.
- Follow-up: ingest the next source that explains why re-applying an unchanged deployment file is not enough to pick up that new image.

## [2026-04-19] ingest | triggering deployment updates

- Trigger: ingest request for the newly added transcript `raw/transcripts/22 - Triggering Deployment Updates.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[22 - Triggering Deployment Updates]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the diagrams, unchanged-apply explanation, and workaround families preserved inline.
- Key synthesis: this source explains that `kubectl apply -f` only reacts to manifest changes, not to newer image contents hiding behind the same image reference, and it compares three imperfect workaround families: manual pod deletion, explicit version tags in config, and version tags plus an imperative deployment-image update command.
- Follow-up: ingest the next source that shows the exact imperative command and the concrete version-tagging workflow the course settles on.

## [2026-04-19] ingest | imperatively updating a deployment's image

- Trigger: ingest request for the newly added transcript `raw/transcripts/23 - Imperatively Updating a Deployment's Image.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[23 - Imperatively Updating a Deployment's Image]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the version-tag build flow, the full `kubectl set image` command, and the verification steps preserved inline.
- Key synthesis: this source turns the previously preferred workaround into a concrete recipe by rebuilding `multi-client` with a version tag such as `v5`, pushing it to Docker Hub, and then using `kubectl set image deployment/client-deployment client=<image>:<tag>` to force the deployment onto the new image version.
- Follow-up: ingest the next source that clarifies how this imperative update workflow should be automated or reconciled with longer-term deployment configuration management.

## [2026-04-19] ingest | the path to production

- Trigger: ingest request for the newly added transcript `raw/transcripts/24 - The Path to Production.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[24 - The Path to Production]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the high-level architecture and staged delivery path preserved inline.
- Key synthesis: this source shifts the course from isolated Kubernetes object mechanics to the full multi-service Fibonacci app architecture, introducing `Ingress Service`, `ClusterIP`, and `Postgres PVC` while laying out the step sequence of local config work, `minikube` testing, GitHub/Travis automation, and eventual cloud deployment.
- Follow-up: ingest the next source that starts the actual config-file buildout for the multi-service app and concretely explains one of the newly introduced objects such as `Ingress`, `ClusterIP`, or the PVC setup.

## [2026-04-19] ingest | a quick checkpoint

- Trigger: ingest request for the newly added transcript `raw/transcripts/25 - A Quick Checkpoint.md`.
- Created or updated: [[25 - A Quick Checkpoint]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the Docker context warning, Docker Compose commands, and browser sanity-check workflow preserved inline.
- Key synthesis: this source inserts a baseline validation step before the Kubernetes migration by making sure the original Docker Compose version of the multi-container app still works on `localhost:3050`, while also warning that Docker Compose must be run against the normal local Docker daemon rather than the Kubernetes node's Docker server.
- Follow-up: ingest the next source that begins the actual manifest-by-manifest migration from the Compose-based app into Kubernetes objects.

## [2026-04-19] ingest | recreating the deployment

- Trigger: ingest request for the newly added transcript `raw/transcripts/26 - Recreating the Deployment.md`.
- Created or updated: [[26 - Recreating the Deployment]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the project cleanup, the new `k8s/` directory, and the normalized `client-deployment.yml` manifest preserved inline.
- Key synthesis: this source begins the real Kubernetes migration by removing old Compose/Elastic Beanstalk deployment artifacts, creating a dedicated `k8s/` folder, and rebuilding the frontend as a three-replica `client-deployment` using the familiar `component: web` selector and port `3000`.
- Follow-up: ingest the next source that creates the matching frontend service and concretely explains `ClusterIP`.

## [2026-04-19] ingest | nodeport vs clusterip services

- Trigger: ingest request for the newly added transcript `raw/transcripts/27 - NodePort vs ClusterIP Services.md` and its related screenshot in `raw/assets/`.
- Created or updated: [[27 - NodePort vs ClusterIP Services]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the comparison diagram and the internal-vs-external networking distinction preserved inline.
- Key synthesis: this source explains that `NodePort` is the outward-facing service type used in the early demos, while `ClusterIP` is the internal-only service type used for object-to-object communication inside the cluster, with `Ingress` becoming the outer entry point in the larger app architecture.
- Follow-up: ingest the next source that creates the actual `ClusterIP` manifest for the frontend and turns this conceptual distinction into a concrete config file.

## [2026-04-19] ingest | the clusterip config

- Trigger: ingest request for the newly added transcript `raw/transcripts/28 - The ClusterIP Config.md`.
- Created or updated: [[28 - The ClusterIP Config]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the full service YAML, supporting service diagrams, and the `port` versus `targetPort` explanation preserved inline.
- Key synthesis: this source turns the earlier `ClusterIP` idea into a real frontend service manifest by creating `client-cluster-ip-service`, keeping the selector on `component: web`, dropping `nodePort`, and using `port: 3000` plus `targetPort: 3000` to define the internal routing path to the `multi-client` pods.
- Follow-up: ingest the next source that shows how `Ingress` will route outside traffic into this internal frontend service or how service names are used for in-cluster communication.

## [2026-04-19] ingest | applying multiple files with kubectl

- Trigger: ingest request for the newly added transcript `raw/transcripts/29 - Applying Multiple Files with Kubectl.md`.
- Created or updated: [[29 - Applying Multiple Files with Kubectl]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the cleanup commands, directory-wide apply workflow, server error, and verification commands preserved inline.
- Key synthesis: this source shows the first real multi-file manifest workflow by deleting the older frontend deployment and `NodePort` service, running `kubectl apply -f k8s`, correcting a deployment YAML mistake after a partial failure, and then confirming that the new three-replica frontend deployment plus `client-cluster-ip-service` are present in the cluster.
- Follow-up: ingest the next source that restores outside access to the app or introduces the next networking object after this internal-only frontend service.

## [2026-04-19] ingest | express api deployment config

- Trigger: ingest request for the newly added transcript `raw/transcripts/30 - Express API deployment config.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[30 - Express API deployment config]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the backend deployment YAML, architecture screenshots, and the deferred environment-variable wiring preserved inline.
- Key synthesis: this source extends the migration from the frontend to the backend by creating `server-deployment` for `multi-server`, reusing the same deployment pattern with `replicas: 3`, `component: server`, and `containerPort: 5000`, while explicitly postponing the Redis and Postgres environment variables needed for the API to run fully.
- Follow-up: ingest the next source that creates the matching backend `ClusterIP` service or wires the backend deployment to Redis and Postgres through environment variables.

## [2026-04-19] ingest | clusterip for express api

- Trigger: ingest request for the newly added transcript `raw/transcripts/31 - ClusterIP for Express API.md`.
- Created or updated: [[31 - ClusterIP for Express API]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the backend service YAML and supporting internal-routing diagram preserved inline.
- Key synthesis: this source creates `server-cluster-ip-service` as the internal service for the Express API, using `type: ClusterIP`, `selector: component: server`, and `port: 5000` plus `targetPort: 5000` so other in-cluster objects can reach the backend pods without exposing the API directly to the outside world.
- Follow-up: ingest the next source that shows how the backend is discovered by name, wired to Redis/Postgres through environment variables, or reached indirectly through `Ingress`.

## [2026-04-19] ingest | combining config into single files

- Trigger: ingest request for the newly added transcript `raw/transcripts/32 - Combining Config Into Single Files.md`.
- Created or updated: [[32 - Combining Config Into Single Files]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the multi-document YAML pattern and the file-organization tradeoff preserved inline.
- Key synthesis: this source clarifies that Kubernetes allows multiple object definitions in one YAML file separated by `---`, using the backend deployment plus backend service as the example, but still recommends the current course convention of keeping one file per object because that makes object lookup and config discoverability clearer.
- Follow-up: ingest the next source that returns from this organizational aside to actual app wiring, especially service discovery, environment variables, or ingress routing.

## [2026-04-19] ingest | worker deployment

- Trigger: ingest request for the newly added transcript `raw/transcripts/33 - The Worker Deployment.md`.
- Created or updated: [[33 - The Worker Deployment]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the worker deployment YAML and the no-service rationale preserved inline.
- Key synthesis: this source creates `worker-deployment` with `replicas: 1`, `component: worker`, and the `multi-worker` image, while making the important architectural point that the worker needs neither a service nor any exposed port because nothing else in the cluster sends direct inbound requests to it.
- Follow-up: ingest the next source that reapplies the new manifests and shows how the worker and backend behave once they actually run in the cluster.

## [2026-04-19] ingest | reapplying batch of config files

- Trigger: ingest request for the newly added transcript `raw/transcripts/34 - Reapplying Batch of Config Files.md`.
- Created or updated: [[34 - Reapplying Batch of Config Files]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the batch apply workflow, `kubectl get` outputs, and backend log inspection preserved inline.
- Key synthesis: this source re-applies the whole `k8s/` directory, shows the new server and worker objects being created while old frontend objects remain unchanged, and then demonstrates that `kubectl logs` can reveal functional dependency failures even when Kubernetes reports the pods as running.
- Follow-up: ingest the next source that adds Redis so the server and worker can start moving from structural validity toward real application wiring.

## [2026-04-19] ingest | volumes vs persistent volumes

- Trigger: ingest request for the newly added transcript `raw/transcripts/35 - Volumes vs Persistent Volumes.md`.
- Created or updated: [[35 - Volumes vs Persistent Volumes]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the normalized Redis deployment YAML, Redis service YAML, and apply-and-verify workflow preserved inline.
- Key synthesis: despite its title, this source is still about adding Redis to the cluster by creating a single-replica `redis-deployment` and a matching `redis-cluster-ip-service` on port `6379`, bringing one more missing internal dependency into the app architecture.
- Follow-up: ingest the next source that adds Postgres, returns to the promised storage concepts, or finally wires the backend and worker to Redis through environment variables.

## [2026-04-19] ingest | last set of config

- Trigger: ingest request for the newly added transcript `raw/transcripts/36 - Last set of config.md`.
- Created or updated: [[36 - Last set of config]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the normalized Postgres deployment/service YAML and apply-and-check workflow preserved inline.
- Key synthesis: this source closes the repetitive manifest-writing phase by adding `postgres-deployment` and `postgres-cluster-ip-service` on port `5432`, while explicitly pointing forward to the more important missing pieces: environment variables and the Postgres PVC.
- Follow-up: ingest the next source that explains why the database needs persistence before moving into the PVC implementation itself.

## [2026-04-19] ingest | the need for volumes with database

- Trigger: ingest request for the newly added transcript `raw/transcripts/37 - The need for Volumes with Database.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[37 - The need for Volumes with Database]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the persistence diagrams preserved inline.
- Key synthesis: this source explains why Postgres cannot safely write data only into the container filesystem, because pod replacement would erase that data, and it motivates persistent storage as the way to reconnect a replacement pod to the same database data.
- Follow-up: ingest the next source that distinguishes Kubernetes `Volume` from `PersistentVolume` and `PersistentVolumeClaim`.

## [2026-04-19] ingest | kubernetes volumes

- Trigger: ingest request for the newly added transcript `raw/transcripts/38 -Kubernetes Volumes.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[38 -Kubernetes Volumes]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the terminology and pod-level-volume diagrams preserved inline.
- Key synthesis: this source clarifies that a Kubernetes `Volume` is a specific pod-level storage object that can survive container restarts inside a pod but not pod replacement, which is why a plain `Volume` is still not enough for durable Postgres data and the course is aiming at `PersistentVolume` plus `PersistentVolumeClaim`.
- Follow-up: ingest the next source that shows the actual PVC configuration and how it connects to the Postgres deployment.

## [2026-04-19] ingest | volume vs persistent volume

- Trigger: ingest request for the newly added transcript `raw/transcripts/39 - Volume vs Persistent Volume.md` and its related screenshot in `raw/assets/`.
- Created or updated: [[39 - Volume vs Persistent Volume]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the lifecycle comparison diagram preserved inline.
- Key synthesis: this source turns the earlier storage warning into a precise lifecycle rule by showing that a plain Kubernetes `Volume` survives container restarts inside a pod but dies with the pod, while a `PersistentVolume` survives both container recreation and pod recreation because it exists outside the pod.
- Follow-up: ingest the next source that explains how a pod actually asks for a persistent volume instead of just describing the storage lifecycle at a high level.

## [2026-04-19] ingest | persistent volume vs persistent volume claim

- Trigger: ingest request for the newly added transcript `raw/transcripts/40 - Persistent Volume vs Persistent Volume Claim.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[40 - Persistent Volume vs Persistent Volume Claim]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the full hard-drive-store analogy, PV/PVC mapping, and static-versus-dynamic provisioning diagrams preserved inline.
- Key synthesis: this source introduces the real request-versus-resource split for Kubernetes storage by treating `PersistentVolumeClaim` as the request layer and `PersistentVolume` as the actual storage resource, while also distinguishing between statically provisioned storage that already exists and dynamically provisioned storage that is created only when a claim is made.
- Follow-up: ingest the next source that finally shows the actual PVC YAML and how the Postgres deployment binds to it in practice.

## [2026-04-20] ingest | claim config files

- Trigger: ingest request for the newly added transcript `raw/transcripts/41 - Claim Config files.md`.
- Created or updated: [[41 - Claim Config files]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the full PVC YAML preserved inline.
- Key synthesis: this source creates the first actual `PersistentVolumeClaim` manifest in the course, naming it `database-persistent-volume-claim` and requesting `ReadWriteOnce` access with `2Gi` of storage, which turns the earlier PV/PVC analogy into a concrete Kubernetes object.
- Follow-up: ingest the next source that explains what the PVC spec fields mean and how Kubernetes interprets those storage requirements.

## [2026-04-20] ingest | persistent volume access modes

- Trigger: ingest request for the newly added transcript `raw/transcripts/42 - Persistent Volume Access Modes.md` and its related screenshot in `raw/assets/`.
- Created or updated: [[42 - Persistent Volume Access Modes]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the access-mode diagram and PVC YAML recap preserved inline.
- Key synthesis: this source explains that the PVC spec is a requirement sheet for storage, clarifying that `ReadWriteOnce` means one node can read and write, `ReadOnlyMany` means many nodes can read, `ReadWriteMany` means many nodes can read and write, and `storage: 2Gi` is simply the requested capacity.
- Follow-up: ingest the next source that explains where Kubernetes actually sources persistent storage from once a claim has been made.

## [2026-04-20] ingest | where does kubernetes allocate persistent volumes

- Trigger: ingest request for the newly added transcript `raw/transcripts/43 - Where does Kubernetes Allocate Persistent Volumes.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[43 - Where does Kubernetes Allocate Persistent Volumes]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the host-disk diagram, storage-class inspection output, and cloud-provider comparison preserved inline.
- Key synthesis: this source introduces the default `StorageClass` as the thing that decides how a PVC gets turned into actual storage, showing that the local environment uses a default `hostpath` class backed by `docker.io/hostpath`, while cloud environments may use very different provisioners such as Google Cloud Persistent Disk, Azure File, Azure Disk, or AWS Block Store.
- Follow-up: ingest the next source that mounts the Postgres PVC inside the deployment template so the database can actually use the storage being requested.

## [2026-04-20] ingest | designating a pvc in a pod template

- Trigger: ingest request for the newly added transcript `raw/transcripts/44 - Designating a PVC in a POD Template.md`.
- Created or updated: [[44 - Designating a PVC in a POD Template]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the normalized deployment fragment and Postgres volume-mount wiring preserved inline.
- Key synthesis: this source finishes the first real persistent-storage configuration by updating `postgres-deployment` so its pod template requests `database-persistent-volume-claim` through `volumes`, then mounts that storage into the Postgres container at `/var/lib/postgresql/data` with `subPath: postgres`.
- Follow-up: ingest the next source that applies the updated config and verifies that the PVC-backed Postgres setup actually works at runtime.

## [2026-04-20] ingest | applying a pvc

- Trigger: ingest request for the newly added transcript `raw/transcripts/45 - Applying a PVC.md`.
- Created or updated: [[45 - Applying a PVC]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the PVC verification commands and object-role explanation preserved inline.
- Key synthesis: this source applies the updated Postgres storage config, verifies that the Postgres pod restarted, and then uses `kubectl get pv` and `kubectl get pvc` to show that a real persistent volume now exists and is `Bound` to `database-persistent-volume-claim`, making the earlier PVC/PV distinction visible in the cluster.
- Follow-up: ingest the next source that wires the application containers to Redis and Postgres through environment variables so the course can finally test real data persistence instead of just object existence.

## [2026-04-20] ingest | defining environment variables

- Trigger: ingest request for the newly added transcript `raw/transcripts/46 -Defining Environment Variables.md` and its related screenshots.
- Created or updated: [[46 -Defining Environment Variables]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the screenshot-driven environment-variable classification preserved inline.
- Key synthesis: this source starts the runtime wiring phase after storage setup by separating ordinary constant environment-variable values from connection-oriented values that behave like service addresses and will be used to connect the app to Redis and Postgres.
- Follow-up: ingest the next source that provides the actual environment-variable keys and deployment-manifest changes for the server and worker.

## [2026-04-20] ingest | adding environment variables to config

- Trigger: ingest request for the newly added transcript `raw/transcripts/47 - Adding Environment Variables to Config.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[47 - Adding Environment Variables to Config]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the worker and server `env` blocks preserved inline.
- Key synthesis: this source adds the first real deployment-level environment-variable config, wiring `REDIS_HOST` and `REDIS_PORT` into the worker, and `REDIS_HOST`, `REDIS_PORT`, `PGUSER`, `PGHOST`, `PGPORT`, and `PGDATABASE` into the server using the in-cluster Redis and Postgres service names.
- Follow-up: ingest the next source that handles `PGPASSWORD` securely instead of storing it directly in the deployment YAML.

## [2026-04-20] ingest | creating an encoded secret

- Trigger: ingest request for the newly added transcript `raw/transcripts/48 - Creating an Encoded Secret.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[48 - Creating an Encoded Secret]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the secret diagrams and secret-creation command preserved inline.
- Key synthesis: this source introduces Kubernetes `Secret` as the right place for `PGPASSWORD`, explains why the value should not be stored in plain text in the deployment config, and uses the imperative command `kubectl create secret generic pgpassword --from-literal PGPASSWORD=postgres` as the course's practical secret-creation workflow.
- Follow-up: ingest the next source that shows how the server deployment reads `PGPASSWORD` from the new secret instead of from a plain `value` field.

## [2026-04-20] ingest | postgres environment variable fix

- Trigger: ingest request for the newly added transcript `raw/transcripts/49 - Postgres Environment Variable Fix.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[49 - Postgres Environment Variable Fix]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the secret-backed `env` entries and apply-error output preserved inline.
- Key synthesis: this source wires the `pgpassword` secret into both the server and Postgres deployments through `valueFrom.secretKeyRef`, which completes the first secret-consumption pattern, and then shows the next concrete failure mode: `kubectl apply -f k8s` fails because numeric env values are still being treated as `int64` instead of strings.
- Follow-up: ingest the next source that fixes the environment-variable typing problem so the updated deployments can be applied successfully.

## [2026-04-20] ingest | environment variables as strings

- Trigger: ingest request for the newly added transcript `raw/transcripts/50 - Environment Variables as Strings.md` and its related screenshots.
- Created or updated: [[50 - Environment Variables as Strings]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the corrected env blocks and successful re-apply output preserved inline.
- Key synthesis: this source fixes the `cannot convert int64 to string` error by quoting numeric-looking env values such as `REDIS_PORT` and `PGPORT`, proving that the prior failure was a YAML typing issue rather than a secret-wiring issue.
- Follow-up: ingest the next source that tests real application behavior now that the env, secret, and deployment config all apply cleanly.

## [2026-04-20] restructure | environment variable visuals cleanup

- Trigger: no new un-ingested transcripts were present, but `raw/assets/Screenshot 2026-04-20 at 10.57.50 AM.png` had been added and was not yet referenced anywhere in the vault.
- Created or updated: [[46 -Defining Environment Variables]], [[wiki/log]], and the two derived notes for `46 -Defining Environment Variables` so the Redis service-connection diagram is embedded directly alongside the earlier classification screenshots.
- Key synthesis: the added diagram makes the earlier “connection-style environment variable” idea more concrete by showing `redis-cluster-ip-service` as the worker’s in-cluster target, which strengthens the bridge between the abstract env-var grouping lesson and the later deployment YAML.
- Follow-up: ingest the next real transcript that moves from cleanly applied config into runtime behavior testing across the full app.

## [2026-04-20] ingest | load balancer services

- Trigger: ingest request for the newly added transcript `raw/transcripts/51 - Load Balancer Services.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[51 - Load Balancer Services]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the service taxonomy and cloud-load-balancer diagrams preserved inline.
- Key synthesis: this source revisits `LoadBalancer` as an older outside-access approach, explains that it exposes one selected set of pods and also causes cloud-provider load-balancer infrastructure to be created, and argues that this app needs ingress instead because it requires more flexible outside routing than one direct target set.
- Follow-up: ingest the next source that introduces ingress and clarifies which ingress-nginx project the course actually uses.

## [2026-04-20] ingest | quick note on ingress

- Trigger: ingest request for the newly added transcript `raw/transcripts/52. - Quick Note on Ingress.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[52. - Quick Note on Ingress]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the ingress routing diagram and project-name comparison preserved inline.
- Key synthesis: this source introduces ingress as the preferred outside-routing tool and makes a critical documentation warning explicit: the course uses `kubernetes/ingress-nginx`, not the separate `nginxinc/kubernetes-ingress` project, even though both projects use very similar names and labels.
- Follow-up: ingest the next source that explains how ingress-nginx setup differs across local and cloud environments.

## [2026-04-20] ingest | ingress setup notes

- Trigger: ingest request for the newly added transcript `raw/transcripts/53 - Ingress setup notes.md` and its related screenshot in `raw/assets/`.
- Created or updated: [[53 - Ingress setup notes]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the environment-specific ingress warning preserved inline.
- Key synthesis: this source warns that ingress-nginx setup differs significantly across local, Google Cloud, AWS, and Azure environments, and it explicitly says the course will cover ingress locally and later on Google Cloud.
- Follow-up: ingest the next source that explains the controller-style mental model behind ingress before installation begins.

## [2026-04-20] ingest | behind the scenes of ingress

- Trigger: ingest request for the newly added transcript `raw/transcripts/54 - Behind the Scenes of Ingress.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[54 - Behind the Scenes of Ingress]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the desired-state, controller, and ingress-nginx diagrams preserved inline.
- Key synthesis: this source explains ingress using the same controller pattern the course already used for deployments: an ingress config declares routing rules, an ingress controller reconciles those rules into reality, and in ingress-nginx the controller and traffic-routing implementation are effectively combined into the same deployment/pod.
- Follow-up: ingest the next source that maps this model onto the actual app architecture and explains the Google Cloud-style ingress-nginx layout.

## [2026-04-20] ingest | more behind the scenes of ingress

- Trigger: ingest request for the newly added transcript `raw/transcripts/55 - More Behind the Scenes of Ingress.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[55 - More Behind the Scenes of Ingress]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the Google Cloud ingress-nginx architecture diagrams preserved inline.
- Key synthesis: this source maps ingress-nginx onto the app architecture on Google Cloud, showing a cloud load balancer outside the cluster, an in-cluster `LoadBalancer` service, the nginx-controller/nginx pod, the client and server services, and a default backend used for health checks, while also leaving open the comparison with a manually managed custom NGINX approach.
- Follow-up: ingest the next source that performs the actual local ingress-nginx setup and turns these ingress concepts into real manifests and commands.

## [2026-04-20] ingest | ingress nginx installation and setup

- Trigger: ingest request for the newly added transcript `raw/transcripts/56 - Ingress Nginx installation and setup.md`.
- Created or updated: [[56 - Ingress Nginx installation and setup]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` that preserve the Docker Desktop setup pointer, the official quick-start URL, and the "If you don't Have Helm" instruction.
- Key synthesis: this source is a short setup bridge rather than a deep transcript; it tells Docker Desktop users to install ingress-nginx from the official quick-start docs and treats the docs page as the source of truth for the actual `kubectl apply` script.
- Follow-up: ingest the next source that creates the first actual ingress manifest and shows how the app's `/` and `/api/` routes are mapped onto in-cluster services.

## [2026-04-20] ingest | creating ingress configuration

- Trigger: ingest request for the newly added transcript `raw/transcripts/57 - Creating Ingress Configuration.md` and its related screenshots in `raw/assets/`.
- Created or updated: [[57 - Creating Ingress Configuration]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the routing diagrams, rewrite behavior, and ingress YAML preserved inline.
- Key synthesis: this source creates the first ingress manifest for the app, routing `/` to `client-cluster-ip-service`, routing `/api/` to `server-cluster-ip-service`, and using `nginx.ingress.kubernetes.io/rewrite-target: /` so the Express server does not have to see the public `/api` prefix.
- Follow-up: the next real step should test ingress locally and reconcile the course's older ingress API shape with modern Kubernetes ingress manifests; `raw/transcripts/58 - Testing Ingress Locally.md` is currently empty.

## [2026-04-20] restructure | ingress format modernization note

- Trigger: no new usable transcript was available because `raw/transcripts/58 - Testing Ingress Locally.md` is still zero bytes, but the vault needed a durable note covering why the course's older ingress YAML fails on the current cluster and what the stable replacement looks like.
- Created or updated: [[kubernetes-modern-ingress-format]], [[57 - Creating Ingress Configuration]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and both derived notes for `57 - Creating Ingress Configuration`.
- Key synthesis: the older ingress lesson still carries the right routing idea, but modern Kubernetes requires `networking.k8s.io/v1`, required `pathType`, nested backend service fields, and preferred `ingressClassName`, while the older annotation-based class wiring and `serviceName` / `servicePort` fields are legacy patterns.
- Follow-up: the next real transcript should ideally show local ingress testing, but even before that the next live-cluster task is to verify that the migrated manifest routes `/` and `/api` correctly with ingress-nginx.

## [2026-04-20] ingest | the deployment process

- Trigger: ingest request for the newly added transcript `raw/transcripts/59 - The Deployment Process.md` and its related screenshot in `raw/assets/`.
- Created or updated: [[59 - The Deployment Process]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the production checklist screenshot preserved inline.
- Key synthesis: this source turns the production story into a concrete operational checklist, tying together repo hosting, CI, Google Cloud project setup, billing, deployment scripts, and GKE Autopilot cluster creation, while also warning explicitly about ongoing cluster cost.
- Follow-up: ingest the next source that explains the CI/CD pipeline itself and preserve the modern GitHub Actions equivalent because the course's original CI platform is Travis.

## [2026-04-20] ingest | ci setup

- Trigger: ingest request for the newly added transcript `raw/transcripts/60 - CI Setup.md` and its related screenshot in `raw/assets/`.
- Created or updated: [[60 - CI Setup]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the Travis flow diagram preserved inline.
- Key synthesis: this source explains the course's full CI/CD logic: authenticate to Google Cloud, build and test images, push them, apply the `k8s/` directory, and then imperatively update deployment image versions because manifest apply alone does not refresh image contents.
- Follow-up: preserve a durable GitHub Actions version of this same flow, because the course logic remains useful even though Travis is no longer the practical CI choice for this vault.

## [2026-04-20] restructure | github actions gke deployment flow

- Trigger: the user explicitly asked for notes that also explain the modern GitHub Actions based CI flow they would actually use, not just the older Travis-based course setup.
- Created or updated: [[github-actions-gke-deployment-flow]], [[60 - CI Setup]], [[59 - The Deployment Process]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and the derived notes for `60 - CI Setup`.
- Key synthesis: the modern translation of the course pipeline keeps the same logic but swaps in current tooling: GitHub Actions, Google Cloud auth via `google-github-actions/auth`, GKE access via `get-gke-credentials`, Docker Hub login via `docker/login-action`, image builds via `docker/build-push-action`, and explicit image-tag deployment updates.
- Follow-up: the next real source should ideally show the course's actual CI file contents or the next live-cluster testing step, but the vault now has a durable modern CI/CD reference even before that source arrives.

## [2026-04-20] ingest | installing google cloud sdk

- Trigger: ingest request for the newly added transcript `raw/transcripts/61 - Installing google cloud sdk.md`.
- Created or updated: [[61 - Installing google cloud sdk]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` with the old `.travis.yml` bootstrap block preserved inline.
- Key synthesis: this source shows how the old Travis pipeline manually installed the Google Cloud SDK, installed `kubectl`, and authenticated with a long-lived `service-account.json` file, while the new notes map that same need onto the modern GitHub Actions pattern of official Google Cloud auth and GKE credentials actions.
- Follow-up: the next useful source should either continue the old Travis file so the remaining deployment logic can be captured, or provide the next live-cluster verification step so the modern GitHub Actions workflow can be grounded against a real deployment run.

## [2026-04-20] restructure | modern ci references

- Trigger: the user explicitly asked that the modernized notes be backed by references, links, and citations instead of only by chat explanations.
- Created or updated: [[github-actions-gke-deployment-flow]], [[kubernetes-modern-ingress-format]], [[60 - CI Setup]], [[61 - Installing google cloud sdk]], [[wiki/index]], [[kubernetes]], and [[wiki/log]].
- Key synthesis: the vault's modern CI/CD and ingress migration notes now point directly to the official Kubernetes, GitHub Docs, Google GitHub Actions, and Docker action references that justify the updated patterns.
- Follow-up: the next modern note worth adding would be a concrete secret/variable checklist for GitHub Actions plus the minimum Google Cloud IAM roles needed for the workflow.

## [2026-04-20] ingest | service account steps for gcp

- Trigger: ingest request for the newly added transcript `raw/transcripts/62 - Service Account Steps for GCP.md`.
- Created or updated: [[62 - Service Account Steps for GCP]], [[kubernetes]], [[wiki/index]], [[wiki/log]], and two derived notes in `derived/notes/` that preserve the old GCP service-account and JSON-key creation flow.
- Key synthesis: this source explains where the old `service-account.json` file came from in the Travis-based deployment setup, while the updated notes make the modern position explicit: GitHub Actions should prefer Workload Identity Federation and treat JSON keys as a fallback only.
- Follow-up: the next modern note worth adding would be a minimum-IAM-permissions guide for the GitHub Actions deployment flow, so the broad course role can be replaced by a tighter one.

## [2026-04-20] restructure | multi k8s google cloud ui setup

- Trigger: the user asked to store the detailed step-by-step setup guidance from the recent chat, especially the Google Cloud UI steps for Workload Identity Federation and service-account permissions.
- Created or updated: [[multi-k8s-google-cloud-ui-setup-for-github-actions]], [[github-actions-gke-deployment-flow]], [[kubernetes]], [[wiki/index]], and [[wiki/log]].
- Key synthesis: the vault now has a project-specific setup note that translates the abstract GitHub Actions modernization guidance into concrete Google Cloud UI actions for `multi-k8s`, including the service-account trust binding, project IAM grant, minimal auth smoke test, common mistakes, and official references.
- Follow-up: the next practical note to add would be the exact final `deploy.yml` for the repo once the cluster name and location are confirmed.

## [2026-04-20] restructure | multi k8s workflow files and yaml patterns

- Trigger: the user asked to store the detailed explanations about where the workflow files should live, whether CI and deploy should be separate, and how to think about writing GitHub Actions YAML from first principles.
- Created or updated: [[multi-k8s-github-actions-files-and-flow]], [[github-actions-yaml-patterns-for-ci-cd]], [[kubernetes]], [[wiki/index]], and [[wiki/log]].
- Key synthesis: the vault now has one note that explains the concrete `multi-k8s` file split and current `ci.yaml` / `deploy.yaml` behavior, and another note that generalizes the train of thought for designing CI/CD workflow YAML from purpose, trigger, runner, permissions, and step order.
- Follow-up: the next practical note to add would be the final `deploy.yaml` that extends the current auth smoke test into Docker build, push, `kubectl apply`, and deployment image updates.

## [2026-04-22] ingest | unique deployment images through helm setup

- Trigger: ingest request for the newly added April 22 transcript files in `raw/transcripts/63 - Unique Deployment Images.md` through `raw/transcripts/68 -.md`.
- Created or updated: [[63 - Unique Deployment Images]], [[64 - Unique Tags for Built Image]], [[65 - Updating the Deployment Script]], [[66 - Configuring the gcloud CLI on the cloud console]], [[67 - Helm and its setup]], [[68 -]], twelve derived notes in `derived/notes/`, [[kubernetes]], [[wiki/index]], and [[wiki/log]].
- Key synthesis: these sources finish the course's production deployment setup sequence by showing that `latest` is not a sufficient rollout identifier, introducing Git SHA tags for deterministic image updates and easier debugging, wiring that strategy into `.travis.yml` plus `deploy.sh`, moving the remaining remote-cluster setup into Google Cloud Shell, and treating Helm as the current ingress-nginx installation path on GKE.
- Key decisions: `[[68 -]]` was ingested as an explicit placeholder source rather than ignored because the file is present but lacks enough evidence for normal synthesis; its page and derived notes record the gap instead of inventing content.
- Follow-up: add the missing April 22 screenshot assets to `raw/assets` if they exist, capture the actual remote `pgpassword` secret-creation step once its source arrives, and replace the placeholder `[[68 -]]` with fuller source material when available.

## [2026-04-22] ingest | deployment mental model and cloud secret options

- Trigger: ingest request for the newly added raw notes `raw/notes/Mental model of deployment to Google Kubernetes engine using GitHub actions.md` and `raw/notes/Managing the secrets for Cloud Deployment..md`.
- Created or updated: [[2026-04-22-mental-model-of-github-actions-gke-deployment]], [[2026-04-22-managing-secrets-for-cloud-deployment]], [[kubernetes-cloud-deployment-secret-management]], [[github-actions-gke-deployment-flow]], [[kubernetes]], [[wiki/index]], and [[wiki/log]].
- Key synthesis: the new notes close two conceptual gaps left by the course material and earlier wiki notes: where the GitHub Actions deployment job actually runs versus where GKE work actually happens, and how cloud-deployment secrets now span a clear maturity ladder from manual cluster creation through GitHub-managed workflow creation to Secret Manager plus an operator.
- Key decisions: the runner-versus-cluster clarification was folded into the existing GitHub Actions deployment analysis instead of creating a second overlapping deployment-flow note, while the secrets note earned its own durable analysis because it synthesizes older course secret material with newer cloud-deployment advice.
- Follow-up: decide which secret-management path this repo should standardize on, and if the repo moves beyond manual Cloud Shell secret creation, store the exact workflow step or controller configuration in the wiki as an implementation note rather than only as a conceptual option list.

## [2026-04-22] query | kubernetes command cheat sheet

- Trigger: request to create a markdown cheatsheet page listing the commands learned so far.
- Created or updated: [[kubernetes-command-cheat-sheet]], `derived/notes/kubernetes-command-cheat-sheet-quick-reference.md`, [[wiki/index]], and [[wiki/log]].
- Key synthesis: the cheatsheet now groups the learned commands into the real operational loops that emerged across the course and later repo notes: `kubectl` apply/inspect, imperative cleanup, image updates, rollout checks, Docker image workflows, Git-SHA versioning, storage inspection, secret creation, GKE targeting, and Helm-based ingress installation.
- Key decisions: the cheatsheet keeps shell commands only and treats older Travis-based GCP bootstrap commands as historical rather than current defaults, while using the newer GitHub Actions and GKE notes to mark the current recommended cloud-deploy sequence.
- Follow-up: once the repo's final `deploy.yaml` is settled, update the cheatsheet so the "current recommended cloud deploy sequence" becomes the repo's exact implementation rather than the current synthesized pattern.

## [2026-04-23] ingest | ingress results through skaffold local development

- Trigger: ingest request for the newly added transcript batch in `raw/transcripts/68 - The result of ingress-nginx.md` through `raw/transcripts/78 - Live sync changes.md`, plus the matching screenshots added under `raw/assets/`.
- Created or updated: [[68 - The result of ingress-nginx]], [[69 - HTTPS SETUP OVERVIEW]], [[70 - Cert Manager Install]], [[71 - How to wire up Cert Manager]], [[72 - Issuer Config File]], [[73 - Certificate Config File]], [[74 - Deploying Changes]], [[75 - Ingress for Config]], [[76 - skaffold overview]], [[77 - Skaffold Config]], [[78 - Live sync changes]], twenty-two derived notes in `derived/notes/` with embedded screenshots where available, [[kubernetes-https-cert-manager-flow]], [[skaffold-local-kubernetes-development]], [[68 -]], [[kubernetes]], [[wiki/index]], and [[wiki/log]].
- Key synthesis: this source batch finishes two major arcs that were previously only partially represented in the vault. The first arc is production HTTPS on GKE: ingress-nginx now has a visible controller and cloud load balancer, HTTPS requires a real domain plus Let's Encrypt validation, cert-manager is installed through Helm, `ClusterIssuer` and `Certificate` become the key cert-manager objects, and ingress is finally updated to force HTTPS and consume the generated TLS secret. The second arc is local-development ergonomics: Skaffold becomes the missing inner-loop tool, the repo is split into shared, dev-only, and prod-only manifest sets, and `skaffold dev` plus file sync restore a faster edit-test cycle.
- Key decisions: screenshot embeds were carried directly into the new source pages and derived notes instead of being left as unattached assets; the older placeholder source [[68 -]] was preserved but updated to reflect that the screenshot now exists locally and that the fuller transcript is now captured by [[68 - The result of ingress-nginx]]; the two new durable syntheses were stored as analysis pages so future questions about HTTPS setup or Skaffold do not require re-reading the whole transcript batch.
- Follow-up: if the repo later reaches the actual domain / DNS wiring and first successful live certificate issuance, add that verification step as a separate source or analysis note so the HTTPS sequence ends with runtime confirmation rather than only manifest and controller setup.
