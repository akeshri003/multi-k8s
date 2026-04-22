# Deploying Changes Explainer

## Source

- Transcript: `raw/transcripts/74 - Deploying Changes.md`
- Wiki source page: [[74 - Deploying Changes]]

## The Real Lesson Here

This is a dependency-order lesson.

The mistake it is trying to avoid is:

- updating ingress to reference a TLS secret
- before cert-manager has created that secret

The safer order is:

1. deploy issuer and certificate objects
2. wait for cert-manager to obtain the certificate
3. confirm the secret exists
4. update ingress to use it

## Why This Matters

The issuer and certificate objects are declarations.

The actual secret is a later effect produced by cert-manager.

So ingress is depending on an output, not just on another YAML file being present.

That is why sequencing matters here more than in many earlier lessons.
