# How to wire up Cert Manager Explainer

## Source

- Transcript: `raw/transcripts/71 - How to wire up Cert Manager.md`
- Wiki source page: [[71 - How to wire up Cert Manager]]

## Key Visual

- ![[raw/assets/Pasted image 20260423002153.png]]

## The Two-File Mental Model

This lesson matters because cert-manager can feel mysterious if it is described as just "install the chart and it works."

It does not work by magic.

It works because you create two object types that describe:

1. where to ask for certificates
2. what certificate to ask for

## Issuer

The issuer is the "who do we talk to?" object.

In this course, that means Let's Encrypt production.

If you wanted, you could create another issuer for staging.

That is why the issuer is its own object instead of a one-off field hidden somewhere else.

## Certificate

The certificate is the "what exactly do we want?" object.

It names:

- the main domain
- any alternate names like `www`
- the secret where the final TLS material should be stored

## Why Kubernetes Objects Are Useful Here

The strong idea in this lesson is that cert-manager is just another controller watching desired state.

You do not call a cert-manager CLI and hand-script every step.

You declare issuer and certificate objects, and the controller reacts.
