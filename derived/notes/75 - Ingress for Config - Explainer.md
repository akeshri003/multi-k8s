# Ingress for Config Explainer

## Source

- Transcript: `raw/transcripts/75 - Ingress for Config.md`
- Wiki source page: [[75 - Ingress for Config]]

## Key Visuals

- ![[raw/assets/Pasted image 20260423005538.png]]
- ![[raw/assets/Pasted image 20260423005600.png]]
- ![[raw/assets/Pasted image 20260423005641.png]]
- ![[raw/assets/Pasted image 20260423005810.png]]
- ![[raw/assets/Pasted image 20260423005820.png]]
- ![[raw/assets/Pasted image 20260423005906.png]]
- ![[raw/assets/Pasted image 20260423010007.png]]

## What Changes In Ingress Once TLS Exists

Earlier ingress only knew how to route requests.

After the certificate work, ingress now also has to know:

- which issuer is tied to the certificate flow
- whether plain HTTP should be redirected
- which hosts are covered
- which secret contains the certificate

So ingress becomes both a routing layer and the final attachment point for TLS material.

## Why `secretName` Must Match The Certificate

The certificate object told cert-manager where to store the certificate.

Ingress must point at that same secret.

If those names drift apart, cert-manager may succeed but ingress still will not serve HTTPS correctly.

## Important Modern Fix

The annotation prefix changed.

So the modern key to remember is:

```yaml
cert-manager.io/cluster-issuer: "letsencrypt-prod"
```

not the older `certmanager.k8s.io/...` form.

That one detail is easy to miss and is one of the highest-value preservation points from this source.
