# Live sync changes Explainer

## Source

- Transcript: `raw/transcripts/78 - Live sync changes.md`
- Wiki source page: [[78 - Live sync changes]]

## What This Proves

The previous lesson defined the config.

This lesson proves the local development loop actually works.

Once `skaffold dev` is running, there are two systems cooperating:

- Skaffold notices the file change and syncs it
- the dev server inside the container notices the changed file and recompiles

That two-step mental model is important.

Skaffold is not itself recompiling the React app.

It is moving the changed file into the container so the app's own tooling can react.

## Why The Fallback Rule Matters

The sync loop only covers the file patterns you listed in the Skaffold config.

If you change something outside those patterns, Skaffold stops doing the fast-path sync and falls back to the slower rebuild path.

So the right way to think about sync rules is:

- synced files = fast inner loop
- everything else = safe but slower rebuild path
