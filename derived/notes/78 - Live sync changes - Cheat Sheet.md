# Live sync changes Cheat Sheet

## Source

- Transcript: `raw/transcripts/78 - Live sync changes.md`
- Wiki source page: [[78 - Live sync changes]]

## Core Command

```bash
skaffold dev
```

## What Happens

- Skaffold applies the selected manifests
- watches project files
- syncs JS / CSS / HTML changes into the running container
- falls back to rebuild if the changed file is outside the sync rules

## Demo Path

- edit `client/src/App.js`
- save
- Skaffold syncs file
- React recompiles
- browser shows update
