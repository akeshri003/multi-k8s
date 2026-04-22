# Issuer Config File Explainer

## Source

- Transcript: `raw/transcripts/72 - Issuer Config File.md`
- Wiki source page: [[72 - Issuer Config File]]

## Key Visual

- ![[raw/assets/Pasted image 20260423003502.png]]

## What The Issuer Is Really Saying

The issuer manifest is basically a standing instruction to cert-manager:

"When I ask for a certificate, use Let's Encrypt production, use this account email, and solve the HTTP challenge through nginx ingress."

That is why this file exists independently from the certificate file.

The certificate file says what cert you want.

The issuer file says how to go get it.

## Why The Update Note Matters

The old transcript still teaches the field meanings well, but the schema changed.

For modern use, the important upgrades are:

- `apiVersion: cert-manager.io/v1`
- keep `kind: ClusterIssuer`
- add the `solvers` block
- specify nginx as the ingress class for HTTP-01 validation

Without that solver wiring, cert-manager does not know how to expose the challenge response through the ingress path the certificate authority will probe.
