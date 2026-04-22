
![[Screenshot 2026-04-17 at 11.26.05 PM.png]]
-: In the last section we had a very long discussion

about what Kubernetes is doing behind the scenes

to deploy our application with some given deployment file,

And there's a couple of takeaways

I want to highlight very quickly.

So, here's a couple of important notes

that I just want to kinda double down on

and make sure are really clear.

So, first off, Kubernetes overall

is a system to deploy containerized apps.

We've been talking about services and pods

and objects, and all this stuff,

but at its core it's all about deploying

containerized applications.

Some of these different objects

that we're going to make use of

have a goal of supporting these containerized applications,

But at the end of the day

it's all about running containers.

Next up, a node is a individual machine,

like a physical computer or a virtual machine

that is going to run some number of containers or objects.

on your computer, with mini cube,

you are currently running exactly one VM

that is behaving as the node.

We also have a master, this is a machine or virtual machine

that has that set of programs that are going to be managing

everything that goes on inside the nodes

associated with your Kubernetes cluster.

Now, the next item on here

is something we didn't quite cover,

Remember, Kubernetes...

no, excuse me, we did, we spoke about Docker Hub.

So, Kubernetes specifically did not build our image

it got the image that we wanted to to deploy

from Docker Hub.

Next big thing is that the master decided

where to run each container.

You and I did not add in any input saying,

"Hey, specifically, I want you to run

this multi-worker container over here or over here."

Now, we absolutely can control

where container gets deployed,

we can add in some configuration

to our deployment files to say,

"Oh, yeah, I want this pod that gets created

to be created on this node or this node."

We had the ability to do that

but by default the master is going to decide

where to deploy all of our different objects for us.

Now, the last two items on here,

and this is the real focus of what I wanna cover

inside this section, is the important stuff.

So, to deploy something we updated the desired state

of the master with a config file.

We passed in this deployment file over here

and the master updated its little list

of responsibilities to say,

"Oh, okay, yeah, I understand,

I'm supposed to be running four copies

of the multi-worker image."

So, the way in which we interacted

with our entire Kubernetes cluster

was to update its desired state.

We said, "This is what we want you to be doing."

We did not specifically say something like,

"Hey, please create this copy of multi-worker,

and this copy, and this copy, and this copy."

Instead, we just very generically say things like,

"I want to run four copies."

And then we hand that directive off to the master

and the master is in charge of making that happen.

Now, the other big important thing here

is to understand that the master is going to work

constantly to meet your desired state,

and that was the entire purpose

of me killing one of those containers

and showing you that Kubernetes

automatically restarted it for us.

So, the master is going to just constantly

churn away at its list of responsibilities

and make sure that our desired state right here,

like the fact that we need to have four copies,

is always going to be met.

Now, again, this entire topic

or this entire idea of having the master

kind of work to meet our desired state,

and the entire idea of us

passing some desired state to the master

in the form of a configuration file

is really key to understanding Kubernetes

and how you and I work with it as a developer.
![[Screenshot 2026-04-17 at 11.30.52 PM.png]]

that are going to expand upon this idea

of us kind of telling Kubernetes what we want

as opposed to us very specifically saying like,

"Hey, please create this pod," or something like that.

Now, this overall topic is kind of a discussion

between two ways of approaching deployment.

We could approach deployment with a imperative style

or a declarative style.

With an imperative style

you and I would issue series of commands

that say specifically like,

"Create this container and create this container,

and then delete that one and then upgrade that one,

and restart that one," or whatever might need to happen.

In the case of a declarative deployment

we say to our master something like,

"You know, it'd be great if you just made

four containers happen

and made four containers happen all the time.

And you know what, master,

you just go ahead you figure out how to deal with that."

So, with the imperative we give very discreet actions,

or excuse me, very discreet commands,

with the declarative we kind of set guidance or a directive,

we say, "Just make this happen for me, would you please?"

Now, just kind of giving you a quick overview right there

is not really enough

so I just wanna show you a couple diagrams

to really drill home the point of the differences

between a imperative deployment and declarative.

Okay, so here's an example of a imperative deployment.

![[Screenshot 2026-04-17 at 11.32.10 PM.png]]

![[Pasted image 20260417233416.png]]

![[Screenshot 2026-04-17 at 11.35.30 PM.png]]

I want you to imagine for a second

that we did not have access

to any of these configuration files or anything like that,

and I want you to imagine that we just had access

to some imaginary Kubernetes command line.

So, I want you to imagine

that you and I have or need to have three containers,

for whatever reason, we decide that our application

has to have three containers right now.

And let's imagine that we don't really have any

Kubernetes commands or anything like that

to get the current status

of how many containers we are running.

So, what would you have to do

to make sure that you have three containers?

Well, chances are you would have to open up some dashboard

on Google Cloud or AWS,

and you would have to look at the overall state

of your application.

So, maybe when you open up your dashboard

you see that you have four containers running,

so what would you have to do to get three containers?

You would have to very distinctly look at your current state

and then figure out some way

to migrate from your current state of four containers

down to one container.

And so you would have to say, "I'll..."

There we go, fix that up really quickly.

You would have to issue some command to say,

"Okay, I need to remove one container

to get from having four down to three."

And so you might issue this imaginary command

that says something like,

"Kubernetes, remove one container."

Now, the important thing

that I want to really drill home here,

this is the really important thing to understand,

is that even though it kinda seems trivial to say,

"Okay, I need three containers and I currently have four,

all I need to do is remove one."

That still required a lot of computation out of you.

You had to understand your desired state,

so your desired state was to have three containers,

you had to do some research to determine your current state

like how many containers you currently have,

and then you had to come up with some migration path,

some series of action that would take you

from your desire, or excuse me, from your current state

to your desired state.

And so you would have to come up

with this migration plan that says,

"Okay, I'm going to remove one container,

and that will take me from my current state

to my desired state of three containers."

Now, I'm sure that sounds like a trivial example like,

"Steven, really?

Figuring out to remove one container is hard?"

Well, let's look at a more complex example,

slightly more complex.

So, in this example of an imperative deployment

let's imagine for a second

that you have a ton of different containers running

and they're all running some very specific version

of the multi-worker image, like maybe version one.

And so you come along and you decide,

"All right, I need to update every running container

to use image version 1.23 instead."

Now, in this case, things would be a lot more challenging.

You would have to open up your dashboard,

you would have to find all of the different containers

that were running the multi-worker image.

So, for example, maybe this container and this container

and this container right here,

maybe these were running a completely different image

and you want to update this one, this one,

this one, this one and this one.

So, maybe you would want to update the versions

used by the green one,

so now determining a migration strategy

is a little bit more complicated.

You have a desired state of running version 1.23

inside of every container,

you have a current state where these

container, container, container, container

are running maybe version one, or whatever it might be.

And so you would have to figure out some way

of isolating just these green containers and saying,

"Okay, I want to pick out like container one, two, three,

seven, 20, right here, and I want to update just those."

And so you would have to figure out

some series of update actions

to just update that very specific set of containers.

And, again, I'm gonna argue that that might be

really challenging for you to do.

So how would we approach a problem like this

with a declarative deployment

rather than an imperative deployment.

![[Screenshot 2026-04-17 at 11.37.45 PM.png]]
![[Screenshot 2026-04-17 at 11.40.11 PM.png]]

Another diagram, so this is how we would do things

with a declarative deployment.

You would say,

"Okay, I need to update all of my containers

running multi-worker to version 1.23."

So, here's what you would do with Kubernetes

in an ideal world.

You would open up your config file,

so like this config file right here,

the same one that we just put together a little bit ago.

You would find the image designation right here

and you would update the tag on this thing to say,

"I specifically want to use version 1.23."

You would then save that file

and then you would say,

"Send that config file off to Kubernetes."

Master would then inspect that file,

it would update its own little list

of responsibilities right here and say,

"Oh, okay, I understand

I need to be running version 1.2.3,"

or whatever it might be.

And then the master is going to automatically find

all the nodes that are not running version 1.23,

it's gonna take those all down,

excuse me, not nodes but pods,

it's gonna take all the pods

that are not running version 1.23 down

and it's gonna create new pods

that are running the correct version.

And now my question to you is,

which of these two deployment processes

would you want to go through?

Where's my diagram?

Over here.

I bet you would probably want to use

this declarative process,

I bet anything you would not want to be responsible

for looking at the current state of your cluster,

finding all the outdated containers

and writing a command to update each of them.

Instead it would be far easier

to just update a single config file,

send that config file to Kubernetes and say,

"Here you go, take care of it.

I don't care, it's up to you figure it out."

And Kubernetes, the master is going to constantly work

to make sure that its configuration is met,

and make sure that whatever the real state of the nodes are

is what it needs to be.

Okay, so this has been a pretty long discussion

and I think that you probably get the idea now

of the differences between a imperative deployment

and a declarative deployment.

So, why am I telling you all this?

This has been like 10 minutes,

I'm really drilling in here,

this has been a very in-depth talk.

So, the reason I'm telling you all this

is that when you start looking at Kubernetes documentation,

blog posts, stack overflow posts, whatever else,

you are going to see some resources out there

recommend that you take a imperative approach.

You're going to see some resources that say,

"Oh, yeah, write in a command into kubectl

that specifically adds a pod

or specifically removes a pod,

or specifically updates a pod," whatever it might be.

But I'm gonna tell you that Kubernetes has the ability

to do things both ways.

So, Kubernetes has a set of commands through kubectl

to take this imperative approach,

that is the set of commands that some blog posts out there

are going to advocate and they're gonna say,

"Oh, yeah, run something like pod, update,"

whatever it might be.

But Kubernetes, or kubectl,

also has a set of commands

to do things through a declarative approach as well.

Throughout this course

you and I are going to constantly strive

to only use the set of commands

that take a declarative approach

to building out a Kubernetes cluster.

That's the whole point,

that's what I'm trying to convey here

in this entire lecture.

I want you to understand

that when you start reading these blog posts,

some of them are going to tell you

to do things in imperative way,

but for any real production deployment,

everything, every engineer out there,

everyone in the community

is always going to advocate taking the declarative approach,

that's the entire point of Kubernetes.

And so it's gonna be kind of up to you,

when you are reading a blog post, to understand that,

"Okay, maybe not all these blog posts

are saying things that are 100% accurate."

And hopefully eventually you'll start to develop a sense

of when a blog post says like,

"Oh, yeah, just go run this command

to update this pod in place."

That's probably something you don't wanna do.

In general, you and I are going to always going to,

excuse me, you and I are going to always

try to do the same thing over and over and over again,

of updating a configuration file

and then sending that config file off to Kubernetes master,

again and again and again.

And so throughout this entire course,

everything that you and I are going to do

from here to the very end,

is going to be all about updating these configuration files

with some new desired state,

and then eventually sending that off to Kubernetes master

and we're just gonna repeat that process

over and over and over.

And that's exactly how you are going to do things

on your own application,

and that's how you're gonna do things

especially in a production environment as well.

All right, so that's enough soap boxing for me for a while

so let's take a quick pause right now,

now that we've gotten all that out out of the way.

Take quick pause right now

and we're going to continue in the next section.

So, quick break, and I'll see you in just a minute.