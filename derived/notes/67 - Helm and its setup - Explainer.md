# Helm and its setup Explainer

## Source

- Transcript: `raw/transcripts/67 - Helm and its setup.md`
- Wiki source page: [[67 - Helm and its setup]]

## Why The Earlier Ingress Work Was Not Enough

Earlier lessons already created the ingress config.

But an ingress config is only rules.

Something still has to enforce those rules inside the cluster.

That "something" is the ingress controller.

So this lesson adds the missing runtime component, not just another YAML file.

## Why Helm Shows Up Here

Helm is presented as the standard tool for installing and managing third-party Kubernetes software.

That makes it a good fit for ingress-nginx, which is bigger than a tiny single-object manifest and comes with its own installation packaging.

## Historical vs Current Truth

This source has two layers:

1. the transcript explains the older conceptual flow of installing Helm and then using it
2. the update note gives the current command that should actually be used now

For operations, the update note is the important part.

## The Current Operational Flow

1. open Google Cloud Console / Cloud Shell
2. run the Helm install-or-upgrade command for ingress-nginx
3. verify that the `ingress-nginx-controller` service exists

That is the moment where the earlier ingress manifest becomes actionable on the production cluster.

## Mental Model

If the ingress YAML is the routing policy, Helm is how this lesson installs the software that can obey that policy.

Without that controller, the ingress object is mostly just an inert declaration.
