# Combining Config Into Single Files Explainer

## Source

- Transcript: `raw/transcripts/32 - Combining Config Into Single Files.md`
- Wiki source page: [[32 - Combining Config Into Single Files]]

## What This Lesson Is Doing

Up to this point, the course has used a very strict rule:

- one object
- one config file

This lesson pauses to say:

that is a choice, not a hard Kubernetes requirement.

So the lesson is not introducing a new Kubernetes object.

It is introducing an alternative way to organize YAML files.

## The Main Idea

Kubernetes allows multiple object definitions inside one file.

To do that, you place one object definition after another and separate them with:

```yaml
---
```

That line means:

this is the end of one YAML document and the beginning of the next one.

So one file can contain:

- one deployment
- one service
- or many objects if you want

## The Example In The Lesson

The transcript uses the backend config as the example.

It says you could take:

- the backend deployment
- the backend `ClusterIP` service

and put both of them into one file such as `server-config.yaml`.

The resulting structure would look like this:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: server-deployment
spec:
  ...
---
apiVersion: v1
kind: Service
metadata:
  name: server-cluster-ip-service
spec:
  ...
```

So instead of two files representing two objects, you would have one file representing one grouped piece of the app.

## Why Someone Might Prefer Combined Files

The argument for combined files is straightforward:

related things stay together.

For example:

- the server deployment
- the server service

are closely related.

So some people prefer to keep them side by side in the same file.

That can make one file feel like "everything for this app component."

The transcript is careful to say this is completely valid.

## Why The Instructor Still Prefers Separate Files

The instructor's main argument is discoverability.

If a new engineer joins the project and wants to change:

- the client `ClusterIP` service

then a separate filename like `client-cluster-ip-service.yaml` makes the answer obvious.

They can find the object's config immediately.

But if the config is grouped into bigger combined files, they first have to know the grouping strategy.

They have to understand something like:

- this service is grouped with that deployment
- both are inside some broader file
- that broader file has a naming convention I need to understand

So the lesson's practical claim is:

separate files reduce lookup friction.

## The Tradeoff

This lesson is really about a tradeoff between:

- grouping related config
- making single-object lookup easier

Combined files help with the first.

Separate files help with the second.

The transcript does not say one approach is universally correct for every team.

It says this is a preference question.

But it is very clear about the course recommendation:

keep using separate files.

## What The Course Chooses

The instructor creates the example combined file only as a demonstration.

Then that file is deleted.

The course continues with four separate files:

- two client files
- two server files

That is important because it means the rest of the course assumes the separate-file approach.

So if you want your vault to track the course faithfully, the durable default should remain:

- one object
- one file

## Why This Lesson Matters

This lesson matters because it tells you that YAML organization is not fixed by Kubernetes itself.

You have some freedom in how you arrange config.

That is useful context because beginners often assume:

- one object per file is required

But really it is just one clear and common organizational strategy.

This lesson also sharpens a good engineering question:

is your repo optimized for grouping related config together, or for helping a person find one specific object quickly?

The course chooses the second.

## Important Caveats

- The lesson only discusses raw YAML organization; it does not go into higher-level tooling or templating systems.
- The example combined file is purely illustrative and is deleted right away.
- The course recommendation is still to keep one separate file per object.

## Best Reuse

Use this note when you want to remember:

- how multiple Kubernetes objects can live in one YAML file
- what `---` does in a manifest file
- why some teams group related objects together
- why this course still chooses one file per object
