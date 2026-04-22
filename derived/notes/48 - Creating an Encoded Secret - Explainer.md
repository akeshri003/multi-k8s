# Creating an Encoded Secret Explainer

## Source

- Transcript: `raw/transcripts/48 - Creating an Encoded Secret.md`
- Wiki source page: [[48 - Creating an Encoded Secret]]

## Key Visuals

- ![[raw/assets/Screenshot 2026-04-20 at 11.09.32 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 11.12.22 AM.png]]
- ![[raw/assets/Screenshot 2026-04-20 at 11.27.47 AM.png]]

## What This Lesson Is Doing

The previous lesson added almost all of the environment variables the app needs.

One value was left out:

`PGPASSWORD`

This lesson explains why.

The password is sensitive, so the course does not want to write it directly into the deployment YAML.

Instead, it introduces a new Kubernetes object:

`Secret`

## What A Secret Is For

The course gives a simple rule:

use a `Secret` for protected values such as:

- database passwords
- API keys
- SSH keys

That is why `PGPASSWORD` belongs here.

By contrast, values like `REDIS_HOST` are not treated as secrets because they are already visible in ordinary service config and are not sensitive in the same way.

## Why This Breaks The Normal Workflow

Most of the course has followed a declarative pattern:

- write YAML
- apply YAML

This lesson makes a deliberate exception.

The secret is created imperatively with `kubectl create secret`.

The reason is practical:

if you had to put the secret value into a normal config file first, you would already be exposing the sensitive value in plain text.

So the course chooses the more manual path for this one object type.

## The Command

The lesson shows this command shape:

```bash
kubectl create secret generic <secret_name> --from-literal=<key>=<value>
```

And the concrete example shown on screen is:

```bash
kubectl create secret generic pgpassword --from-literal PGPASSWORD=postgres
```

The important parts are:

- `secret generic`
  - create a generic key-value secret
- `pgpassword`
  - the name used to refer to the secret later
- `--from-literal`
  - provide the secret data directly in the command
- `PGPASSWORD=postgres`
  - the key/value pair being stored

## Why The Secret Name Matters

The secret name is not just cosmetic.

The deployment will need to reference this secret later when it wants to read the password into an environment variable.

So the command is doing two things:

- storing the sensitive value
- giving that stored value a stable object name inside the cluster

## The Operational Cost

The lesson also points out an important tradeoff.

Because the secret is created imperatively, it must be created manually in every environment.

That includes production.

So this is more secure than hardcoding the password in the YAML, but it also creates a new deployment step you must remember.

## What Is Still Missing

This lesson still does not show the final consumption step.

It explains how to create the secret, but it does not yet show:

- the `env` entry that reads from the secret
- the deployment update that uses it

That is the next missing piece in the sequence.

## Best Reuse

Use this note when you want to remember:

- why `PGPASSWORD` is handled differently from the other env vars
- what a Kubernetes `Secret` is for
- the exact `kubectl create secret generic` pattern
- why secret creation becomes a manual environment-setup step
