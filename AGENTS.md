# AGENTS.md

This repository is a personal LLM wiki. The agent's job is to turn raw inputs into a persistent, interlinked, continuously improving markdown knowledge base.

The wiki is not a chat transcript dump. It is a maintained synthesis layer.

## Primary Objective

Build and maintain a durable second brain that:

- preserves important knowledge from raw sources
- compounds over time instead of re-deriving everything at query time
- surfaces contradictions, uncertainty, and open questions
- stays navigable through strong indexing, linking, and logging

The user curates sources and guides direction. The agent performs the maintenance work.

## Non-Negotiable Rules

- Never modify files under `raw/`.
- Treat `raw/` as immutable source material.
- Do not write derived transcripts, summaries, or prep notes back into `raw/`.
- Put agent-created prep artifacts in `derived/`.
- Always preserve traceability from wiki claims back to sources.
- Do not invent facts. If a claim is not directly supported, label it as inference.
- When sources conflict, preserve the conflict instead of flattening it away.
- Prefer incremental edits to existing wiki pages over unnecessary rewrites.
- Keep the wiki useful to a future reader who has not seen the chat.
- Read `wiki/index.md` before most substantial query or maintenance work.
- Check recent entries in `wiki/log.md` before making major updates.

## Repository Model

There are four main layers:

1. `raw/`
   Immutable source material.
2. `derived/`
   Agent-created prep artifacts derived from raw sources.
3. `wiki/`
   LLM-authored markdown pages that synthesize, organize, and connect knowledge.
4. `schemas/`
   Templates, conventions, and operating guidance for maintaining the wiki.

This repo also contains a few legacy compatibility notes at the root and in older folders. Treat `wiki/index.md`, `wiki/log.md`, and `wiki/overview.md` as the canonical control files.

## Folder Structure

### `raw/`

- `raw/inbox/`
  New unprocessed sources land here first.
- `raw/articles/`
  Web articles, blog posts, essays.
- `raw/papers/`
  Research papers and reports.
- `raw/books/`
  Book notes, chapter text, excerpts.
- `raw/videos/`
  Local video files or markdown descriptors for remote videos.
- `raw/transcripts/`
  Podcast, meeting, lecture, and interview transcripts.
- `raw/notes/`
  Personal notes, journal fragments, loose observations.
- `raw/assets/`
  Images and supporting media referenced by sources.
- `raw/library/`
  Legacy or uncategorized source storage already present in this vault. Keep immutable like the rest of `raw/`.

### `derived/`

- `derived/transcripts/`
  Agent-created transcripts, caption normalizations, or stitched subtitle files.
- `derived/notes/`
  Agent-created timestamp notes, outlines, prep notes, and ingestion scaffolds.
- `derived/assets/`
  Agent-created low-level artifacts such as extracted frames when useful.

### `wiki/`

- `wiki/index.md`
  Main navigation layer. Read this first for most queries.
- `wiki/log.md`
  Append-only operational history.
- `wiki/overview.md`
  Current scope and state of the wiki.
- `wiki/sources/`
  One page per ingested source.
- `wiki/topics/`
  Multi-source syntheses organized by topic.
- `wiki/entities/`
  Pages for recurring named things: people, companies, methods, products, places, concepts.
- `wiki/analysis/`
  Durable outputs created from queries: comparisons, memos, timelines, decks, charts.
- `wiki/maps/`
  Navigational hub pages for clusters of related material.
- `wiki/questions/`
  Open questions, research leads, and unresolved tensions.

Legacy directories under `wiki/` may remain for compatibility. Prefer the taxonomy above for new work.

### `schemas/`

- `schemas/templates/`
  Reusable page templates.
- `schemas/checklists/`
  Workflow checklists for ingest, video ingest, and maintenance.

## Page Taxonomy

Use the smallest page type that fits the information cleanly.

### Source pages

Belong in: `wiki/sources/`

Purpose:

- preserve what this source says
- capture key claims
- connect the source into the rest of the wiki

### Topic pages

Belong in: `wiki/topics/`

Purpose:

- summarize the current understanding of a subject
- aggregate evidence from multiple source pages
- note disagreements and uncertainty

### Entity pages

Belong in: `wiki/entities/`

Purpose:

- consolidate mentions scattered across sources
- maintain a stable page for things that recur over time

### Analysis pages

Belong in: `wiki/analysis/`

Purpose:

- preserve high-value outputs that should not vanish into chat history

### Map pages

Belong in: `wiki/maps/`

Purpose:

- act as an overview or table of contents for a region of the wiki

### Question pages

Belong in: `wiki/questions/`

Purpose:

- preserve open loops
- guide future ingestion and research

## Naming Conventions

- Use kebab-case filenames for new files wherever practical.
- Prefer concise, stable names.
- For source pages, prefer `YYYY-MM-DD-short-title.md` when the source date is known.
- For evergreen pages, prefer semantic names like `sleep-optimization.md` or `vannevar-bush.md`.
- Rename pages only when it materially improves long-term clarity.

## Frontmatter Conventions

Use lightweight YAML frontmatter when helpful.

Recommended fields:

- `title`
- `type`
- `status`
- `created`
- `updated`
- `source_path`
- `source_date`
- `tags`
- `related`

Do not add decorative metadata with no operational value.

## Writing Standard

Write for clarity, retrieval, and accumulation.

### Style

- Prefer plain, precise prose.
- Prefer short sections over large walls of text.
- Use bullets when scanning matters.
- Use dates concretely when known.
- Use markdown links generously but intentionally.

### Claims

- Distinguish clearly between source-backed claims and synthesis.
- Preserve the strongest available formulation without overstating certainty.
- If a point is tentative, say why.
- If evidence is weak or incomplete, record that explicitly.

### Quotes

- Preserve only high-value quotes.
- Do not overload pages with long excerpts.
- Use quotes when wording matters or when a formulation should be preserved exactly.

## Derived Notes Policy

When the user asks to "make notes" for a source, the default output should be two derived notes in `derived/notes/`:

1. a concise cheat sheet
2. a more explanatory note

The explanatory note should:

- assume the reader may be a novice
- use concrete examples from the source
- use analogies and mental models that map to modern software engineering where possible
- explain not only what a term means, but how to think about it

Do not place agent-authored explanatory notes into `raw/notes/`; keep them in `derived/notes/` unless the user explicitly provides their own raw note material.

## Link Policy

- Every substantive page should link outward to related pages.
- Important pages should receive inbound links from at least one index, map, or related page.
- Link specific phrases, not generic filler.
- Add reciprocal links when they materially improve navigation.
- If a concept is repeatedly mentioned but lacks a page, create one when useful.

## Index Policy

`wiki/index.md` is the first file to consult during most queries.

Keep it:

- compact
- current
- skimmable
- comprehensive enough to orient search without opening dozens of pages

Each listed page should usually include:

- link
- one-line summary
- optional status or source count when informative

## Log Policy

`wiki/log.md` is append-only and chronological.

Each entry must start with:

`## [YYYY-MM-DD] operation | title`

Supported operation labels:

- `bootstrap`
- `ingest`
- `query`
- `lint`
- `restructure`

Each log entry should include:

- what triggered the action
- which pages were created or updated
- key synthesis or decisions
- any follow-up questions or gaps

## Standard Workflows

### Workflow: Ingest

Use this when a new source is added.

Steps:

1. Identify the source file in `raw/`.
2. Determine the source type.
3. If the source is audio or video, prepare the smallest useful derived artifact set in `derived/`.
4. Read the source fully enough to understand its structure and central claims.
5. Determine source metadata: title, source type, author if known, date if known, source path.
6. Create or update a source page in `wiki/sources/`.
7. Extract candidate entities, topics, and open questions.
8. Update relevant topic pages.
9. Update relevant entity pages.
10. Create a question page if the source introduces unresolved tensions worth tracking.
11. Update `wiki/index.md`.
12. Append a structured entry to `wiki/log.md`.

Ingest output should usually touch more than one file. A source page alone is rarely enough.

### Workflow: Video Ingest

Use this when the source is a tutorial, lecture, interview, walkthrough, or other video.

Steps:

1. Identify the original source in `raw/inbox/` or `raw/videos/`.
2. Capture minimal source metadata: title, creator, URL if present, publication date if known, and source path.
3. Prepare derived artifacts in `derived/` as available: transcript, captions, outline, timestamp notes, screenshots.
4. Mark transcript quality clearly: human transcript, platform captions, machine transcript, or partial notes only.
5. Build the source page around what the video actually teaches, not around generic metadata.
6. Preserve important timestamps when they materially improve retrieval.
7. If no transcript is available, ingest only to the level justified by the available evidence and record the gap explicitly.
8. If the environment lacks transcription tooling, prefer using user-provided captions, notes, or a URL descriptor rather than fabricating detail.
9. Update the smallest useful set of topic, entity, question, index, and log files.

### Workflow: Query

Use this when the user asks a question about the knowledge base.

Steps:

1. Read `wiki/index.md`.
2. Identify the most relevant pages.
3. Read the minimum useful set of pages.
4. Synthesize an answer with citations to wiki pages.
5. If the answer is durable, file it to `wiki/analysis/`.
6. If the wiki changed, update `wiki/index.md` and `wiki/log.md`.

### Workflow: Lint

Use this periodically or on request.

Check for:

- stale summaries
- conflicting claims
- weak source attribution
- orphan pages
- duplicate pages
- unlinked entities
- topics that need map pages
- unresolved questions that now have answers

When fixes are made, log them.

## First-Ingest Heuristics

When the wiki is young:

- keep structure simple
- create only the pages that clearly earn their existence
- favor one strong topic page over five thin pages
- favor one source page plus a couple of connected evergreen pages

Do not explode the ontology too early.

## Source Page Standard

A source page should usually contain:

- short metadata block
- summary
- key claims
- important evidence or examples
- related entities
- related topics
- tensions or open questions

For video sources, include when useful:

- original video path or URL
- creator or channel
- transcript quality
- important timestamps
- what was demonstrated versus merely asserted

## Topic Page Standard

A topic page should usually contain:

- current synthesis
- key subthemes
- evidence across sources
- disagreements or caveats
- linked source pages
- open questions

## Entity Page Standard

A typical entity page should contain:

- concise description
- why it matters in this wiki
- claims or facts tied to source pages
- related topics and analysis

## Analysis Page Standard

A typical analysis page should contain:

- the question
- answer or conclusion
- evidence from the wiki
- uncertainties
- links to relevant pages

## Question Page Standard

A question page should contain:

- the question
- why it matters
- current partial answer, if any
- relevant pages
- next sources or investigations to pursue

## Maintenance Priorities

When time is limited, prioritize:

1. source attribution
2. strong topic pages
3. entity consolidation
4. index quality
5. log quality
6. secondary polish

## Expected Agent Behavior

The agent should behave like a careful wiki maintainer, not a generic assistant.

That means:

- read before writing
- update multiple connected files when warranted
- preserve chronology through the log
- preserve discoverability through the index
- preserve uncertainty instead of hiding it
- optimize for future reuse, not just the current conversation

## Default Assumption

If the user says "ingest this", the agent should:

- process the source
- if needed, prepare derived artifacts in `derived/`
- create or update the source page
- update any obviously relevant topic or entity pages
- update the index
- append a log entry

Do the maintenance work automatically unless the user explicitly asks for a narrower operation.

If the source is a video, "ingest this" means:

- identify the video file or URL descriptor in `raw/`
- prepare a transcript or the best available substitute in `derived/`
- create a source page that records both the original source and the derived evidence used
- update the relevant topic, entity, index, and log pages
