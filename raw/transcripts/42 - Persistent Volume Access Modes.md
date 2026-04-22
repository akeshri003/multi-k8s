```database-persistent-volume-claim.yaml
apiVersion: v1

kind: PersistentVolumeClaim

metadata:

	name: database-persistent-volume-claim

spec:

	accessModes:

		- ReadWriteOnce

	resources:

		requests:

			storage: 2Gi
```

A volume claim is not an actual instance of storage. 
![[Screenshot 2026-04-19 at 11.57.16 PM.png]]
  
![Lecture thumbnail](https://mp4-cdn77.udemycdn.com/2018-08-24_22-45-47-baf1ef389194b777030fc757f79a4238/2/thumb-sprites.jpg?secure=l4_v9JnXCRYgeguxO50xsQ%3D%3D%2C1776639256)

3:29 / 3:31

Instructor: In the last section,

we put together our first persistent volume claim.

Now, before we talk about what the spec stuff

inside of here is, I wanna give you a very quick reminder.

Remember that a volume claim

is not an actual instance of storage.

A volume claim is something that we are going to attach

to a pod config.

So back in our example over here,

we would take this volume claim that says,

hey, I can get you a 500 gigabyte hard drive

and we attach it to our pod config.

Then at some point in time,

we take that pod config and we hand it off to Kubernetes.

Kubernetes is going to see that storage option,

it's gonna see that volume claim,

and it's going to try to find

either a statically provision, persistent volume

or a dynamically provision, persistent volume

to meet the requirements of that claim.

So all to config that you and I have now

added in underneath the spec section is saying,

that if we attach this claim to a pod,

Kubernetes must find an instance of storage,

like a slice of your hard drive

that meets these requirements.

So with that in mind,

let's talk about what an access mode is.

All right, so access modes.

We get three different types of access modes.

The one that we used is ReadWriteOnce.

ReadWriteOnce means that we want to get some

instance of storage like a slice of your hard drive

that can be used by a single node at a time.

And to be precise, that single node can use it

and both read and write information to that volume.

We also get access to an access mode of ReadOnlyMany,

which means that multiple nodes at the same time

can read information from this persistent volume.

And then finally, ReadWriteMany

means that we can both read and write information

to this persistent volume by many nodes at the same time.

Now, again, I want to really, really highlight the fact

that the access mode that we put in right here says,

if you attach me to a pod config

and then hand me off to Kubernetes,

Kubernetes is going to have to find an instance

of storage that supports this access mode.

All right, so let's now move on to resources down here.

Now, I bet you can really guess what's going on here.

So with storage, we're essentially saying

that Kubernetes is going to have to find a storage option,

either one that has been provisioned ahead of time

or one that is going to be created on the fly

that has at least two gigabytes of space.

Or to be exact,

it has to have exactly two gigabytes of space.

We could very easily change this number around,

we can say, 1000 gigabytes or four, six,

whatever you want it to be.

But for us, two is way more than what is needed

to put together our very simple Postgres database.

In fact, we're gonna use like fractions of a kilobyte.

So two gigabytes is way overkill for our application

but I just wanna give you an example

with a gigabyte option here.

Okay, so these two options right here,

pretty straightforward in nature.

Again, they are saying, hey, Kubernetes,

you have to find a storage instance

that has this access mode

and it has to have exactly two gigabytes worth of storage.

Now, there's a couple of other options

so we can add into this file that we did not

because we're going to rely upon the defaults

but these other storage options that we did not list out

are pretty important to understand.

So let's take a quick break.

When we come back to the next section,

we'll talk about some other possible options

that we could put into this spec if you had need to do so.