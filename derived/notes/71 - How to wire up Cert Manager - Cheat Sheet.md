# How to wire up Cert Manager Cheat Sheet

## Source

- Transcript: `raw/transcripts/71 - How to wire up Cert Manager.md`
- Wiki source page: [[71 - How to wire up Cert Manager]]

## Key Visual

- ![[raw/assets/Pasted image 20260423002153.png]]

## Two Objects You Need

- `ClusterIssuer`
- `Certificate`

## What Each One Means

- issuer = which certificate authority to talk to and how
- certificate = what cert you want and which secret should store it

## Durable Idea

- cert-manager watches these objects and acts automatically after they are created
