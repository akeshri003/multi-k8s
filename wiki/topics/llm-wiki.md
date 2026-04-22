---
title: LLM Wiki
type: topic
status: active
created: 2026-04-17
updated: 2026-04-17
tags:
  - llm-wiki
  - knowledge-management
related:
  - wiki/sources/2026-04-17-llm-wiki-pattern.md
aliases:
  - LLM Wiki
---

# LLM Wiki

## Current Synthesis

An LLM wiki is a persistent markdown knowledge base that an LLM maintains over time as new sources arrive. The core design move is to make the wiki itself the durable memory layer instead of relying only on repeated retrieval from raw inputs ([[2026-04-17-llm-wiki-pattern]]).

## Key Subthemes

- Ingest is the central operation: each source should update the wiki, not just become retrievable.
- Durable query outputs should be filed back into the repository when they add lasting value.
- Indexing and logging are core infrastructure, not optional polish.
- The schema matters because it turns the agent from a generic assistant into a disciplined maintainer.

## Evidence Across Sources

- [[2026-04-17-llm-wiki-pattern]] defines the architecture, rationale, and maintenance loop.

## Disagreements Or Caveats

- The pattern only compounds if maintenance quality stays high as the repository grows.
- The right amount of metadata, granularity, and automation still needs tuning through use.

## Open Questions

- At what scale will file-based navigation need supplemental search tooling?
- Which topic types will earn dedicated map pages versus staying in the main index?

## Related Pages

- [[wiki/overview]]
- [[schemas/checklists/ingest-checklist]]
