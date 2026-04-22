---
title: Kubernetes HTTPS Cert Manager Flow
type: analysis
status: active
created: 2026-04-23
updated: 2026-04-23
tags:
  - kubernetes
  - https
  - tls
  - cert-manager
  - ingress
related:
  - wiki/topics/kubernetes.md
  - wiki/sources/68 - The result of ingress-nginx.md
  - wiki/sources/69 - HTTPS SETUP OVERVIEW.md
  - wiki/sources/70 - Cert Manager Install.md
  - wiki/sources/71 - How to wire up Cert Manager.md
  - wiki/sources/72 - Issuer Config File.md
  - wiki/sources/73 - Certificate Config File.md
  - wiki/sources/74 - Deploying Changes.md
  - wiki/sources/75 - Ingress for Config.md
---

# Kubernetes HTTPS Cert Manager Flow

## Question

What is the full HTTPS setup flow in this Kubernetes sequence, and how do ingress, cert-manager, issuer, certificate, and the TLS secret fit together?

## Short Answer

The flow is:

1. install ingress-nginx
2. install cert-manager
3. create a `ClusterIssuer`
4. create a `Certificate`
5. let cert-manager complete the Let's Encrypt validation flow
6. wait for the TLS secret to be created
7. update ingress to use that secret and force HTTPS

The important dependency is that ingress should not point at the TLS secret until cert-manager has actually created it.

## Mental Model

- ingress-nginx is the traffic entry layer
- cert-manager is the certificate automation controller
- issuer says where and how to ask for a certificate
- certificate says what cert to get and what secret should hold it
- ingress finally uses that secret to serve HTTPS

## Why A Real Domain Is Required

The certificate authority validates domain ownership by probing a challenge path under the claimed domain.

So the practical prerequisites are:

- buy or already own a real domain
- point DNS at the cluster's external entry point

## Object-Level Breakdown

### Ingress NGINX

This creates the controller and cloud-entry path that later certificate validation relies on.

The visible result is:

- ingress controller exists
- default backend exists
- external IP / load balancer exists

### Cert Manager

This is the automation layer that watches certificate-related objects and acts on them.

Installing cert-manager does not itself produce a site certificate.

It only installs the controller that can later react to the issuer and certificate objects.

### ClusterIssuer

The issuer tells cert-manager:

- use Let's Encrypt
- use this ACME server URL
- solve the HTTP challenge through nginx ingress

### Certificate

The certificate tells cert-manager:

- which domain names need to be covered
- which issuer to use
- which secret should store the resulting TLS material

### Ingress TLS Wiring

Once cert-manager has obtained the certificate and created the secret, ingress gets updated with:

- `cert-manager.io/cluster-issuer`
- `nginx.ingress.kubernetes.io/ssl-redirect: "true"`
- `tls.hosts`
- `tls.secretName`

That is the final step that turns plain HTTP routing into HTTPS serving.

## Practical Sequence For This Vault

1. confirm ingress-nginx is installed and has an external IP
2. install cert-manager with Helm
3. apply issuer manifest
4. apply certificate manifest
5. wait for cert-manager to create the TLS secret
6. update ingress to reference that secret
7. verify host routing and HTTPS redirect behavior

## Common Failure Boundaries

- no real domain or DNS not pointed correctly
- ingress-nginx installed but app ingress rules not yet applied
- old cert-manager API versions used in manifests
- old annotation prefix `certmanager.k8s.io/...` used instead of `cert-manager.io/...`
- ingress references a secret name that does not match the certificate object
- ingress updated before the secret exists

## Durable Takeaway

The HTTPS path is not "install cert-manager and you are done."

It is a controller chain with strict dependencies:

- traffic entry must exist
- certificate automation must exist
- issuer and certificate declarations must exist
- TLS secret must be produced
- ingress must be updated to consume that secret
