![[Screenshot 2026-04-19 at 11.03.01 PM.png]]
![[Screenshot 2026-04-19 at 11.04.03 PM.png]]
![[Screenshot 2026-04-19 at 11.04.59 PM.png]]
![[Screenshot 2026-04-19 at 11.06.34 PM.png]]

-: In the last section,

we spoke about how we might use volumes

as a way of persisting data outside of a container

when that data is something that we care about

and we want to have the persisted across restarts

or termination of a given container,

specifically or most especially ones

that are running a database of some sort.

Now, in this section,

we're gonna start talking about volumes

in the world of Kubernetes.

The first thing I wanna clarify

is a little piece of terminology,

and it's actually the word "volume" itself.

I wanna tell you a little bit

about what the word "volume" means

in the world of Kubernetes.

All right, so first things first

is a little discussion about terminology.

And the last section, I was using the word "volume"

over and over and over again.

When I was using the word volume in the last section,

it was kind of as a generic term

in container terminology world,

and it was a reference to some type

of data storage mechanism

that allows a container to store data

outside of its own little file system.

However, in the world of Kubernetes

the term "volume" is a reference

to a very particular type of object,

an object in the same sense,

that a deployment is an object, or a service is an object.

So, in the world of Kubernetes,

we can write a configuration file

that will create something called a volume.

In Kubernetes that object is a something

that allows a container to store some persistent data

at the pod level.

In addition to the use of volumes,

we also have access to two other types

of data storage mechanisms,

both something called a PersistentVolumeClaim

and a PersistentVolume.

So, in this section, I want to expand upon what a volume is

in the world of Kubernetes, just a little bit,

so that you understand that

when we use the word volume with Kubernetes,

it's a reference to a very particular thing

and it's very different

than referring to something called a PersistentVolume

or a PersistentVolumeClaim.

Now, we're not gonna make use of volumes

for everything that we're doing.

We're not gonna use volumes at all.

I just want you to understand

that this is a completely separate thing.

And so, when you start to look at documentation

and you see documentation that says,

"Oh, yeah, create a volume."

You have to create a distinction

between what a volume is and the PersistentVolume is.

Otherwise, when you're reading that documentation

you're gonna get really, really confused.

Okay, so with that in mind, this section,

we're gonna look at a diagram

that explains exactly what a volume object type is

in Kubernetes.

All right.

So, I want to again consider our Postgres deployment.

Let's imagine we have a deployment

that manages a single pod,

and that pod has a single Postgres container.

When we create a volume in Kubernetes,

we are creating a little kind of data storage pocket

that exists or is tied directly to a very specific pod.

So, you'll notice in this diagram,

I'm reflecting this volume right here

that's going to store some amount of data

as being kind of like inside the pod.

It's essentially kinda like belongs to

or is associated to the pod in some fashion.

Now, this volume can be accessed

by any container inside of the pod.

So, for example, if we had a Postgres container running,

it could store all of its data inside of this volume

that belongs to the pod itself.

Now, the benefit to using this volume here

is that if this container, like this one specifically,

ever dies or crashes

and gets restarted as a completely new container,

like so, and let's imagine that we get

a second Postgres container and the old one gets killed,

so I'm going to mark it as red right there.

Then this new Postgres container has access

to all of that data in that volume.

And so, that might sound, like,

exactly what we were just talking about in the last section.

You know, we're essentially saying,

"Oh, yeah, new container gets created."

Well, the new container gets access to that same volume

and it has access to all the data

that the previous container had access to as well.

However, here's the downside,

the volume is tied to the pod.

And so, if the pod itself ever dies,

the volume dies and goes away as well.

So, a volume in Kubernetes will survive

container restarts inside of a pod.

But if the pod itself, for whatever reason,

ever gets recreated or terminated, deleted,

whatever happens,

then the pod and the volume inside of it,

poof, totally gone.

And then of course, the deployment would kick in,

and recreate that pod with a volume inside of it.

So, as you might guess, in the world of Kubernetes,

a volume is really not appropriate for storing data

for a database.

It definitely works in the sense

that the container can restart

but were still are weak or were still kind of vulnerable

to anything going wrong at the pod level itself.

All right, so that's why we're not going to use volumes.

And just in so, you know, in real life

as I say, volume, I'm doing, like, you know,

double quotes around myself to say, you know,

specifically a Kubernetes volume.

That's why we're not using a Kubernetes volume.

All right, so now that we've clarified

that little piece of terminology.

We're gonna come back to the next section

and we'll talk more about exactly a PersistentVolume is

and a PersistentVolumeClaim.

So, quick pause, and I'll see you in just a minute.