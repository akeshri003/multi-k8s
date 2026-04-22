![[Screenshot 2026-04-20 at 12.00.34 AM.png]]

`kubectl get storageclass`  --> your list of options to make a persistent storage class.
![[Screenshot 2026-04-20 at 12.03.21 AM.png]]

`kubectl describe kubectl`
![[Screenshot 2026-04-20 at 12.04.37 AM.png]]
![[Screenshot 2026-04-20 at 12.04.54 AM.png]]

Instructor: In the last section,

we spoke about some of the different config

that we put into our persistent volume claim.

Now at the end of last section

I had mentioned that there are some other possible options

so we could put inside of here.

We did not put these options in because

we are going to rely upon the defaults for each of these

but there's one option in particular

that I want to mention because it might be very important

for you to use at some point in time in the future.

Okay?

So I want you to imagine kind of what happens

behind the scenes when we hand that persistent volume claim

off to Kubernetes.

And so if we look back at the diagram over here

remember at some point in time we hand that pod config off

and Kubernetes sees, oh, they want that 500 gigabyte option.

So Kubernetes has to like go and find

either that pre configured pre-created ahead of time,

persistent volume, or make one on the fly.

So think about what's going on behind the scenes there.

We're essentially saying to Kubernetes,

Hey I need one gigabyte worth of storage.

Can you get me something that meets these requirements?

That's what the persistent volume claim is really doing.

So Kubernetes gets that

and then we're going to imagine what exactly happens

on your computer when you run this stuff.

We can kind of imagine that Kubernetes probably

says something like, All right, well

I'm on this developer's personal laptop

or maybe I'm on their personal desktop.

Not a lot of storage options available here.

Like pretty much all I've got to put any information

in is your hard drive.

So on your computer

why default Kubernetes is gonna say, You know what?

I'm gonna make a slice of your hard drive.

That is one gigabyte large.

And so we can imagine on your hard drive

we get this one gigabyte little piece

of your hard drive that is going to be a persistent volume

and that persistent volume gets handed

back over to your pod.

So on your computer,

when you ask Kubernetes for some amount of storage,

there's really just one place

for Kubernetes to allocate that storage.

It's just your hard drive

and there's really not a lot of other options.

Now, you can actually see

in the Kubernetes config where it decides to

make this the default where it decides to say

I'm just gonna put it on your hard drive.

To do so, you can flip on over to your terminal

and run Cube ctl get storage class.

So when you run get storage class

you're gonna see all the different options on your computer

that Kubernetes has for creating a persistent volume.

Right now, we have a single option called standard.

It is the default option.

So if we do not specify a storage class

with our persistent volume claim

the standard option will be used by default

and the provisioner or essentially

how Kubernetes is going to decide how to provision

or create this persistent volume is

by using the mini cube host path option.

And I want you to also do a cube CTL describe

storage class as well,

and you'll see some more information

about that option right there.

So when you do so, it tells you in very broad terms,

Yes, we have a provisioner that is mini cube, host path.

Mini cube host path means exactly what we saw over here.

Mini cube host path means that when

we ask Kubernetes to create this persistent volume

it's going to look on the host machine to mini cube

and it's gonna make a little slice

of space on your personal hard drive.

All right, so at this point, this probably seems,

you know at least somewhat palatable, like, yeah

Kubernetes need some space.

It's gonna try to claim some space

off of your computer or your personal hard drive.

But things start to get way more complicated when

we start to move our Kubernetes cluster

from your computer into a cloud environment.

So at some point in time, you and I are going to

move our application over to a cloud provider.

And when we are working with a cloud provider

we get a tremendous number of different options

of where some hard drive space

or essentially file system space can be sourced from.

So in a cloud or production environment, if we say, Hey

I need one gigabyte of storage to Kubernetes

Kubernetes is gonna then say,

Great, which one do you want?

I got like a billion options available.

And so just about every cloud provider

that you're going to work with is going to

have some different solution for storing information.

And you need to tell Kubernetes

unless you wanna use the default option

which of these different options you want to use.

So if you are hosting an application on Google Cloud

the Google Cloud service called Persistent Disc is

going to be used to store information

with your persistent volume.

If you're on Microsoft Azure

you can either use Azure file or Azure disc.

If you're on AWS, you will use AWS Block Store.

Now, there's many other options that are available

in a cloud environment.

You can see some

of the other options at this address right here.

I actually already opened up that page.

So on that page you'll see storage classes.

And if you scroll down to Provisioner,

the provisioner is essentially what determines

how this space you're asking for gets created.

And so these are all the different options

that you have available.

You can look that list and decide on which one you want

but I can tell you that by default, most of the time

you're probably going to want whatever the default is

for your particular cloud provider, which on, say

Google Cloud, it's gonna be persistent disc

on AWS it's going to be block Store.

Now, when you create your Kubernetes cluster

on a cloud provider like Google Cloud or AWS,

one of these services is going to be set up

by default for you.

So in other words, right now inside

of our local environment, we are setting

up this persistent volume claim without designating

a storage class name.

And we did not designate that option

because we are relying upon the default.

And like I just said, the default for us is to

create a little slice on our hard drive to use

for this persistent volume.

But when you push your application up to Kubernetes

on Google Cloud or AWS,

the standard option is not going to be mini cube host path.

Instead, the standard option, or the default option for use

on AWS or Google Cloud will be automatically configured

for you to be either Google Cloud Persistent Disc

or AWS Block store.

So long story short here

what I'm trying to tell you is that when you ask Kubernetes

for some amount of storage on your local machine

it's just gonna take a slice of your hard drive.

When you are in a cloud environment

it's going to choose one of these default options for you

unless you very specifically tell it to use something else.

For most applications, you know,

I don't really want to make a broad claim here

but I'm gonna tell you that

if you've got a normal application that just

has a traditional database like Postgres or MySQL

you're probably going to be okay using the default options

which are probably going to usually be

Google Cloud persistent Disc or AWS Block Store.

But I just want you to know that this option is available

and you can customize where Kubernetes is going to

look to create this little file system to allocate

to your pod.

All right, So enough on all this

I think we've spoken about persistent volume claims enough.

So we've now created our persistent volume claim

which is essentially advertising an option that can be used

for storage by all the different pods in our application.

So last thing we have to do in the next section

we're going to open up our Postgres deployment

and we're going to update the pod config in here and tell it

that when this pod is created, it's going to need to look

at all the different storage options that are available

and advertised by this persistent volume claim

and make sure that it says,

Oh yeah I want this very specific volume claim

that's going to give me a two gigabyte storage option.

So quick pause and I'll see you in just a minute.