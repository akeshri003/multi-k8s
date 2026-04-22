# Cert Manager Install Explainer

## Source

- Transcript: `raw/transcripts/70 - Cert Manager Install.md`
- Wiki source page: [[70 - Cert Manager Install]]

## Why This Exists

The HTTPS overview explains the challenge-response dance with Let's Encrypt.

This lesson installs the software that will automate that dance for you.

That software is cert-manager.

## What Cert Manager Gives The Cluster

Before cert-manager, your cluster has ingress and routing, but no built-in certificate workflow.

After cert-manager, the cluster can watch certificate-related objects and do the repetitive work of:

- requesting certificates
- handling validation flow
- storing the resulting secret
- renewing later

## Most Important Operational Detail

The update note matters more than the older spoken command.

For this repo, the durable install shape is:

- add Jetstack Helm repo
- update Helm repos
- install cert-manager chart
- create the `cert-manager` namespace
- install the CRDs

If you forget the CRDs, the later certificate-related object types will not exist cleanly on the cluster.
