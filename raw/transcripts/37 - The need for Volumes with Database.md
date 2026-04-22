
PVC - Persistent Volume Claim - sharing the filesystem of the host system with that of the container or the vm.

![[Screenshot 2026-04-19 at 10.55.14 PM.png]]

## What is Volumes?
![[Screenshot 2026-04-19 at 10.55.33 PM.png]]![[Screenshot 2026-04-19 at 10.57.37 PM.png]]
-: In the last section we finished

up with our POSTGRES deployment file

and the associated cluster IP service as well.

So that's pretty much it for the boring configuration files,

at least the ones around the cluster IP service

and deployments.

We're still going to write

out a couple of different config files,

but they're going to be much different

than the ones we put together so far, because

they're going to serve dramatically different purposes

than the services and deployments we've taken a look at.

In this section in particular, we're gonna start looking

at what a POSTGRES PVC thing down here is.

First beginning by getting a better understanding

on why we need this PVC thing at all.

So just so you know, PVC stands for persistent volume claim.

Now the word volume in here, is the same type of volume

that we had worked back with in the world of Docker

and Docker Compose a while ago.

You might recall that we had previously made use

of volumes in order to share the file system

of a host operating system or a host machine,

with the file system inside of a container.

And so we had previously used it with Docker Compose

when we were working on that create React app,

we had wanted to make sure that every time

we changed the source code of our project

on our local machine, it somehow updated the files

inside of the container as well.

Now, to give you a good idea

of what a persistent volume claim is,

I first wanna do a very quick review on what a volume is

and why we need a volume at all with POSTGRES in particular.

Because if we just start talking about

what a volume claim is without really understanding

and remembering what a volume is a lot of this stuff

won't make sense.

So in this section, quick review on volumes and why

we need one with POSTGRES.

All right, now, I just took the diagram

we were looking at a second ago,

like this big one right here in particular,

the deployment and POSTGRES pod piece

and I blew it up to be its own kind of diagram right here

as you see.

So we still have the deployment, which creates a pod,

and inside that pod we have a single POSTGRES container.

And inside that container is a file system, totally

isolated to just be accessible by the container itself.

Now, I want you to recall that POSTGRES is a database

and the POSTGRES database, very similar to many other types

of databases, though not relevant for Redis

because that is specifically an in-memory data store,

POSTGRES takes in some amount of data and writes it

to a file system.

So we can imagine that a request to write some data

or essentially save some data with POSTGRES,

comes into the container.

POSTGRES is going to process it and then eventually say

"Okay, I want to store this information on a file system

or a hard drive of sorts."

So inside that container, we can imagine that there is

that file system and some amount of data

is being stored on it.

Now here's the thing to keep in mind

about any file system that is maintained inside

of a container.

If we ever had a situation where for, if any reason you

can possibly imagine, this POSTGRES container

or the pod kind of wrapping it and managing it crashes,

then everything over here gets a hundred percent lost,

including the file system that exists inside

of the POSTGRES container.

So if we just use our copy of POSTGRES

as it stands right now, like this single deployment

with this pod inside of it and no associated bells

or whistles or volumes and stuff associated with it,

if we just write data to POSTGRES

and then that pod or that container eventually crashes

that entire pod is going to be deleted by the deployment,

and a brand new pod is going to be created in its place.

And this new pod is going to have absolutely

no carryover of data.

So none of the data on the file system

or the original container, gets brought over

to this new container or the pod that wraps it.

So essentially the instant that the deployment

starts up a new pod, we'd lose all the data

sitting inside of our database.

And as you might guess, that is a hundred percent

not something we want to ever deal with.

We never want to experience any type of data loss

with any database such as POSTGRES.

So that's the issue.

If we just let POSTGRES save all of its data

inside the file system maintained by the container,

we're gonna lose it as soon as this pod crashes

or the container crashes, and we have to absolutely assume

that that might happen at some point in time.

So how are we gonna solve this?

Well recall that is where volumes come in.

Now, when we had previously used volumes, it was all

in the context of kind of being allowed to make changes

to our source code files

and have them show up inside the container.

But we can also make use of volumes to have a consistent

file system, that can be accessed by a database

such as POSTGRES.

So we can now imagine that with a volume in place

that is running on a host machine,

if we have a request to write data that comes

into the container, POSTGRES is going to think

that it's writing it to a file system that exists

inside the container, but in reality

it's going to be a volume that actually exists outside

on the host machine.

The results of this is that if our original pod

or the POSTGRES container inside of it crashes

for whatever reason whatsoever,

the deployment is going to delete that thing

and then create a brand new pod

with a brand new copy of POSTGRES inside of it.

But we're going to make sure

that this new copy of POSTGRES that gets created

gets access to the exact same volume.

And so we'll have access to all of the data

that had been written by the previous copy of POSTGRES

that already existed.

So that's the idea behind a volume, and that's how

we're going to allow ourselves to save some amount of data

with a database, but not have to worry about all the data

inside there being deleted anytime that the container has to

be restarted or crashes or whatever reason,

whatever might happen.

Now, one quick thing that I wanna mention here.

You'll notice that inside of our POSTGRES deployment

you'll recall we put down replicas of one right here.

Now, I want you to recall that I told you,

"Yeah, we can set up POSTGRES to have like some amount

of replication or clustering that's going to

improve the availability and performance of our database."

Just to make sure it's really clear, if we just

like bump that up to Replica's,

like to right there,

we would end up with a situation like this,

where we have two pods that might be accessing

the same volume.

Having two different databases access the same file system

without them being aware of each other

and have them very distinctly cooperating with each other,

is a recipe for disaster.

So at no point in time are you ever going to want to just

arbitrarily dial up replicas to two, like so,

in attempt to have two copies of POSTGRES

accessing the same volume.

Now that's not just isolated to the world of POSTGRES

many other databases, you're gonna find the same problem.

So for whatever reason you want to scale up your copy

of POSTGRES and make it more available

by having more copies of it running or whatever it might be,

you have to go through some additional configuration steps,

besides just incrementing that Replica's number

right there.

So again, I just wanna make sure that was really,

really, really clear.

Okay, so now that we recall why we make use of volumes

and why a volume is so important to use with a database,

let's continue in the next section where we're going to

start to talk about exactly what a persistent volume claim

is and how it's going to assist us in setting up a volume

for our POSTGRES pod.

So quick break and I'll see you in just a minute.
