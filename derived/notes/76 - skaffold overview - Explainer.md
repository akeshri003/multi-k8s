# skaffold overview Explainer

## Source

- Transcript: `raw/transcripts/76 - skaffold overview.md`
- Wiki source page: [[76 - skaffold overview]]

## Key Visual

- ![[raw/assets/Pasted image 20260423010827.png]]

## Why This Tool Shows Up Late In The Course

Earlier lessons were about understanding Kubernetes objects.

Now the problem shifts from "how does the cluster work?" to "how do I iterate locally without hating my life?"

That is the gap Skaffold fills.

## The Important Comparison

Docker Compose felt convenient because local files could be mounted directly into a development container.

Plain Kubernetes does not give you that same ergonomic loop automatically.

So without Skaffold, the local dev cycle gets much slower.

## The Two Modes

Mode one is the safe, generic path:

- rebuild the image
- redeploy it

Mode two is the faster path:

- push only changed files into the running container
- let the app's own dev tooling notice and reload

That second mode is the whole reason this tool matters for the React and Node setup in this repo.
