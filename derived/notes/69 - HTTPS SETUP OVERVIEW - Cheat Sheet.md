# HTTPS SETUP OVERVIEW Cheat Sheet

## Source

- Transcript: `raw/transcripts/69 - HTTPS SETUP OVERVIEW.md`
- Wiki source page: [[69 - HTTPS SETUP OVERVIEW]]

## Key Visuals

- ![[raw/assets/Pasted image 20260423001046.png]]
- ![[raw/assets/Pasted image 20260423001125.png]]

## Preconditions

- buy a real domain
- point it at the cluster later

## Core Flow

1. cluster asks Let's Encrypt for a certificate
2. Let's Encrypt says: prove you own the domain
3. Let's Encrypt requests `/.well-known/...`
4. cluster responds correctly on that route
5. Let's Encrypt issues the certificate
6. renew again later

## Durable Idea

- HTTPS here is really domain validation plus automated certificate issuance
