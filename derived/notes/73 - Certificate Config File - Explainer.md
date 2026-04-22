# Certificate Config File Explainer

## Source

- Transcript: `raw/transcripts/73 - Certificate Config File.md`
- Wiki source page: [[73 - Certificate Config File]]

## Key Visuals

- ![[raw/assets/Pasted image 20260423004252.png]]
- ![[raw/assets/Pasted image 20260423004503.png]]

## What This File Adds

If the issuer file answers "where should cert-manager go?", the certificate file answers "what certificate should come back?"

That includes three important decisions:

- what secret name should hold the final TLS material
- what issuer should be used
- what domain names the cert must cover

## Why `secretName` Matters

This is the bridge into the ingress step.

Later, ingress will need to know which secret contains the certificate and private key.

So `secretName` is not just storage trivia.

It is the name the rest of the stack will point at.

## Why `dnsNames` Exists

A certificate is not automatically valid for every possible hostname.

If you want both:

- `yourdomain.com`
- `www.yourdomain.com`

then both names need to be captured in the requested certificate shape.

## Modernization Boundary

The older transcript shows extra ACME config under the certificate.

For the modern setup in this repo, that older block should be removed and the cleaner `cert-manager.io/v1` schema should be used instead.
