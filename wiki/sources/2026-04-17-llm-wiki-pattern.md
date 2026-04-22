---
title: LLM Wiki Pattern
type: source
status: active
created: 2026-04-17
updated: 2026-04-17
source_path: raw/library/2026-04-17-llm-wiki-idea.md
source_date: 2026-04-17
tags:
  - llm-wiki
  - knowledge-management
related:
  - wiki/topics/llm-wiki.md
---

# LLM Wiki Pattern

## Summary

This source proposes replacing query-time-only retrieval with an LLM-maintained, persistent markdown wiki that accumulates knowledge over time. The core claim is that the durable artifact should be the wiki layer, not transient chat answers or repeated retrieval from raw documents.

## Key Points

- The system has three layers: immutable raw sources, an LLM-written wiki, and a schema file that defines maintenance rules.
- Ingestion is the core operation: every new source should produce a source page plus targeted updates across existing concept, entity, overview, and analysis pages.
- Query answers should be generated from the wiki first and saved back into the wiki when they are durable.
- Periodic linting is necessary to find contradictions, stale claims, orphan pages, and missing cross-links.
- `index.md` and `log.md` are first-class navigation tools rather than incidental notes.

## Notable Details

- The proposal explicitly contrasts this pattern with standard RAG systems that rediscover the same knowledge on each query.
- Obsidian is positioned as the browsing and inspection layer, while the LLM acts as the maintainer.
- The approach is intended to generalize across personal knowledge management, research, reading, business documentation, and other long-horizon contexts.
- The document recommends small local tools only when scale demands them; until then, file structure plus index-driven navigation is sufficient.

## Connections

- [[llm-wiki]] captures the core topic introduced by this source.
- [[overview]] is the operational overview implementing this pattern in this vault.
- [[schemas/checklists/ingest-checklist]] operationalizes the ingest loop described in the source.

## Open Questions

- What page templates become most useful once the vault expands past the first dozen sources?
- At what scale does the current index-first approach stop being sufficient?
- Which metadata fields will prove worth the maintenance cost in practice?
