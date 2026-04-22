
![[Pasted image 20260417182435.png]]

Instructor: In the last section,

we said that Kubernetes is a system

for running many different types of containers

over multiple machines

and we choose to use it if we have an application

that specifically requires us to run

multiple different types of containers.

If you are planning on creating an application

that would largely just have

one type of container inside of it,

Kubernetes might not be the best solution for you.

Now in this section, we're gonna continue a little bit more

by talking about how we use Kubernetes

in a development or production environment.

And then we're going to start setting up Kubernetes

on our local machine.

All right, so when we work with Kubernetes,

there is a very large distinction

between using it in a development environment,

as in on your local computer and in a production environment

where you would expect some outside traffic or users

to visit your application.

In a development environment,

we make use of Kubernetes

by using a program called minikube.

Minikube is a command line tool whose sole purpose is

going to be to set up a tiny little Kubernetes cluster

like the one you see right here on your local computer.

We'll talk about what minikube does in just a little bit,

but for right now,

just understand that it's a program that is used

to set up Kubernetes on your local machine.

When we start using Kubernetes in a production capacity,

we very frequently make use of what are called

managed solutions.

Managed solutions are references to outside cloud providers,

such as Google Cloud or Amazon AWS

that will set up an entire Kubernetes cluster for you

and take care of a lot of very low level tasks

that are required to get everything working

the way you expect in a secure fashion.

And so if you're making use of Kubernetes on AWS,

you would be making use of something called

Amazon's Elastic Container Service with Kubernetes,

which is abbreviated as EKS.

And if you make use of it on Google Cloud,

you would be making use of something called

Kubernetes Engine or GKE for short.

You always also have the option

to set up a Kubernetes cluster on your own

as a sort of do it yourself option.

And so you do not have to use these managed solutions

that set up everything for you.

But when you're first getting started with Kubernetes,

it's without a doubt,

far easier to go with one of these managed solutions

until you actually really have

a deep understanding of Kubernetes

and how it all works together.

All right, so the big takeaway in this section is that

in a development environment,

you and I are going to be

making use of something called minikube.

And a production environment,

we very frequently make use of these managed solutions.

![[Screenshot 2026-04-17 at 6.28.52 PM.png]]

you're going to be making use of a program called minikube

Minikube is gonna create a Kubernetes cluster

on your local machine.

And so behind the scenes,

it's essentially going to create a virtual machine

whose sole purpose is going to be to run

some number of containers.

Minikube is a program that is used to

create this virtual machine

or this single node on your computer.

In order to interact with this thing,

you and I are going to be using a program called kubectl.

Kubectl is the program that is used

to interact with a Kubernetes cluster in general

and manage what all the different nodes are doing

and what different containers they are running.

So the reason I'm showing you this diagram right here

is that we're gonna start using a lot of different programs

to work with Kubernetes.

And it's going to very quickly become a little bit confusing

as to what program is doing what.

And so as everything moves forward from this point on,

I want you to remember that we use kubectl

anytime that you want to tell a virtual machine or a node

what set of containers it's supposed to be running

or essentially manage what the node is doing.

And we use the program minikube

just to create and run a Kubernetes cluster

on your local machine.

When you start moving to a production environment,

this minikube thing down here falls away.

So we only make use of minikube in a local environment.

However, kubectl is going to be a program that you use

both locally and production.

So even when you start to deploy your application

on a Kubernetes cluster off to say Google Cloud,

we will continue to use kubectl.

All right, so here's what we're going to do

to set up Kubernetes on our local machine.

We're gonna first install that kubectl program.

We then have to install a virtual machine driver

that's going to allow us to create that virtual machine

that we mentioned just a second ago on your local computer.

Now I want you to remember

back to our entire discussion about Docker a long time ago,

you'll recall that we had said that

when we installed Docker for Mac or Docker for Windows,

it was technically creating a virtual machine

at the same time.

So that's essentially the same thing that's happening

in the world of Kubernetes.

The only difference is that

Docker for Mac or Docker for Windows

install that virtual machine for you

and all the associated software with it.

In the world of Kubernetes, not quite so automatic.

You and I have to install a little bit of software

that's going to allow Kubernetes to create,

or minikube in this case

to create a virtual machine for you.

Now to install this virtual machine,

we're going to be installing a program

in particular called VirtualBox.

So after we install a kubectl and VirtualBox,

we're then going to install minikube.

Again this is the piece of software

that it allows you to create a Kubernetes installation

on your local computer.

All right, so that's it.

Let's take a quick pause right here.

When we come back the next section,

we're gonna start going to this setup process

on macOS and Windows.

So quick break and I'll see you in just a minute.