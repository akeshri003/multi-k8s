# Deploying Changes Cheat Sheet

## Source

- Transcript: `raw/transcripts/74 - Deploying Changes.md`
- Wiki source page: [[74 - Deploying Changes]]

## Deploy Order

1. apply issuer manifest
2. apply certificate manifest
3. let cert-manager request and store the certificate
4. only then update ingress to use the TLS secret

## Durable Idea

- do not point ingress at a TLS secret before that secret actually exists
