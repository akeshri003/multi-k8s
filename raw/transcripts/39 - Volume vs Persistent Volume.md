![[Screenshot 2026-04-19 at 11.08.28 PM.png]]

Instructor: In the last section,

we spoke about what a volume is in the role of Kubernetes.

We're now going to start to talk about

these two other types of objects that we can create as well,

a persistent volume claim and a persistent volume.

We'll first begin by comparing and contrasting

the differences between a persistent volume and a volume.

So quick diagram to figure out what

the difference is between these two are.

All right, so in this diagram.

on the left hand side,

we have a Postgres deployment with a volume.

That's exactly what we were talking

about in the last section.

We had said that a volume

is going to be some long term storage that is tied to a pod.

Whenever the pod is created, the volume is created.

If a container crashes for any reason,

the volume will stick around.

The volume will persist.

And so we had said that

if we end up with a new Postgres container,

for whatever reason,

no problem, it can freely connect back

to this existing volume that had been created.

And so we can kind of imagine that the old container

might fall away and it gets replaced with this new one.

Still gets access to all that volume data.

However, if the entire pod crashes for any reason,

everything inside the pod is lost, including that volume.

All right, so that's a volume.

That's what we spoke about in the last section.

So now let's compare and contrast that

against a persistent volume.

With a persistent volume,

we are creating some type of long term durable storage

that is not tied to any specific pod

or any specific container. So you can kind of imagine

that the persistent volume is outside the pod,

completely separate from the pod.

If this container crashes for any reason

or if it needs to be recreated for any reason,

no problem whatsoever.

The old container will fall away

and the new one can connect

to that persistent volume that exists outside the pod.

Now, let's consider the other case.

The case in which maybe a pod

needs to be deleted or recreated

for some fashion, for some reason, excuse me.

So we can kind of imagine

that if this pod crashes entirely and gets recreated,

no problem whatsoever, the old pod falls away,

it completely disappears but the persistent volume

is still going to stick around.

When the new pod is created

with a new copy of pod Postgres,

that container is going to be able

to connect to that persistent volume

that exists outside of the pod.

So that's a big difference between a normal volume

and a persistent volume.

Essentially, we're talking about the life cycle

of the volume itself.

With a normal volume,

it's tied to the life cycle of the pod.

With a persistent volume,

it's going to last for all time,

or essentially until you and I,

as developers, or as the administrators,

decide to delete it for some reason.

But with a persistent volume, we can recreate a container,

we can recreate a pod, no problem.

The volume is still going to stick around

with all the data that you would expect to have.

All right, So that's the differences

between a persistent volume and a volume.

So let's take another quick break right here

and we'll talk about

what the differences are between a persistent volume

and a persistent volume claim.

So, quick pause and I'll catch you in just a second.