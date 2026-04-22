---
title: Wiki Overview
type: overview
status: active
created: 2026-04-17
updated: 2026-04-17
---

# Wiki Overview

## Scope

This repository is a personal LLM-maintained second brain. The working model is:

- `raw/` stores immutable source material
- `derived/` stores agent-created prep artifacts
- `wiki/` stores durable synthesis
- `schemas/` stores templates and workflow guidance

## Current State

- The operating rules live in [[AGENTS]].
- Navigation starts in [[wiki/index]].
- Chronology lives in [[wiki/log]].
- One foundational source has been ingested: [[2026-04-17-llm-wiki-pattern]].
- One topic page currently anchors the model: [[llm-wiki]].

## Working Defaults

- Read before writing.
- Prefer updating existing topic pages over spawning thin new pages.
- Keep source attribution explicit.
- Preserve uncertainty and conflict instead of flattening it away.
- File durable query outputs into `wiki/analysis/` when they should persist.

## Immediate Next Step

Place the next source into the appropriate `raw/` subfolder, then ask for an ingest. The agent should create or update the source page, topic or entity pages as needed, refresh [[wiki/index]], and append an entry to [[wiki/log]].
