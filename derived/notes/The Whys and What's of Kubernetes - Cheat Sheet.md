# The Whys and What's of Kubernetes Cheat Sheet

## Source

- Transcript: `raw/transcripts/The Whys and What's of Kubernetes -.md`
- Embedded diagram: `raw/transcripts/Pasted image 20260417182116.png`
- Supplementary screenshots:
  - `raw/assets/Screenshot 2026-04-17 at 6.16.56 PM.png`
  - `raw/assets/Screenshot 2026-04-17 at 6.17.38 PM.png`

## Core Idea

Kubernetes is useful when an application needs to run different kinds of containers, in different quantities, across multiple machines.

## Key Terms

- `cluster`: the overall Kubernetes system made of a control machine plus worker machines
- `master`: the control plane in this source's terminology; it decides what runs on each node
- `node`: a virtual machine or physical computer that runs containers
- `container`: an individual running workload on a node
- `load balancer`: receives external requests and sends them into the cluster

## Main Claims

- A cluster is made of a master and one or more nodes.
- Nodes can run different container images and different numbers of containers.
- Developers interact with the master, not with each node manually.
- The master receives desired state and tells the nodes what to run.
- A load balancer sends outside traffic into the nodes.
- Kubernetes is overkill if the app only repeats the same simple container layout without needing differentiated scaling.

## Diagrams To Remember

- ![[Pasted image 20260417182116.png]]
  This is the main cluster diagram referenced directly by the transcript.
- ![[Screenshot 2026-04-17 at 6.16.56 PM.png]]
  This shows the earlier Elastic Beanstalk-style scaling layout used as contrast.
- ![[Screenshot 2026-04-17 at 6.17.38 PM.png]]
  This is another capture of the cluster diagram with request, load balancer, master, and nodes.

## Retrieval Cues

- "master plus nodes form a cluster"
- "nodes can run different containers"
- "load balancer sends traffic into the cluster"
- "use Kubernetes when workloads differ by type and count"
