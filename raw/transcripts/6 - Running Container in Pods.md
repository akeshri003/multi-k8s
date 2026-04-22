
![[Screenshot 2026-04-17 at 7.48.36 PM.png]]

  
-: In the last section, we expanded upon the API version

and kind properties inside of both our config files.

We're now going to start to talk about what a pod is

and then we'll very quickly

talk about what a service is as well.

Okay, so a pod.

All right, quick diagram.

Let's find it here, here we go.

All right, so just a moment ago

when we went through that setup process

to install Minikube and kubectl on your local machine.

Remember we installed Minikube

and then we ran that command Minikube, start.

When we ran that command

it created a new virtual machine on your computer.

We referred to that virtual machine

that is now running on your computer as a node,

that node is going to be used by Kubernetes

to run some number of different objects.

Now, one of the most basic objects that you and I

are going to create throughout this course

is something that we're refer to as a pod.

And so when we start to load up

this configuration file right here into kubectl,

we're going to push into kubectl

and it's going to create a pod

inside of that virtual machine

that is running on your computer

and remember, we, we refer to that thing as a node.

Now the pod itself is essentially a grouping of containers

with a very common purpose.

Now, you might be wondering why we're making a pod

that has a grouping of containers

when just a moment ago I had told you,

"Oh yeah, we want to get something running as a container."

Well, in the Kubernetes world

there is no such thing

as just creating a container on a cluster.

Back with Elastic Beanstalk, back with Docker Compose,

we were creating containers willy nilly all day,

no issue what's whatsoever,

but in the world of Kubernetes,

we do not have the ability to just run

one naked single container by itself

with no associated overhead.

The smallest thing that you and I can deploy is a pod.

###  Why is that, why would we wanna make a pod at all??

![[Screenshot 2026-04-17 at 8.43.26 PM.png]]
![[Screenshot 2026-04-17 at 8.45.37 PM.png]]
So we're always going to be declaring, or excuse me,

we're always going to be deploying containers within a pod.

That's the smallest thing that we can deploy

to run a single container.

Now, you might be wondering why is that?

Why would we make a pod at all?

Well, let's talk about why we would make a pod.

So the requirement of a pod

is that we must run one or more containers inside of it.

Now, when I say one or more containers

you might very quickly think back

to the multi-container application that we put together.

As a, let's pull up a quick diagram of that thing,

I've got one around here somewhere, here we go.

So back on the multi-container application we put together

we had our Elastic Beanstalk

and it was running multiple different containers

all in this single little grouping.

And so when I tell you that inside of a pod

we will run one or more containers,

you might be thinking,

"Oh, okay, that's it, like game over.

We're gonna take our four containers or four images

right here and create four of these containers

inside of this one single pod."

Well, that would definitely be an option,

but it's not what a pod is meant to do.

The purpose of a pod is meant to group, it is, excuse me,

the purpose of a pod is to allow the grouping of containers

with a very similar purpose,

where containers that absolutely,

positively must be deployed together

and must be running together in order for our application

to work correctly.

Now, even when I say that you might be thinking,

"Well, Steven, back here on this application,

we had to have the express server deployed with NGINX

up here to serve the React application

or respond to API calls."

So you might be thinking,

"Yeah, you know, this thing had to have

all these different containers put together

to work correctly."

Well, actually, that's not a hundred percent accurate.

You see, back on this application,

if our worker container over here crashed for any reason

the rest of the application would still function A-okay.

If the NGINX React server right here failed,

well everything would still pretty much work correctly.

If the express API closed down,

well everything else would at least kind of still function.

In other words,

if we take away the vast majority of these containers,

the rest of the containers inside the group

are still going to generally function the way we expect.

It's definitely not ideal,

but they would definitely continue to work.

Now, in the world of a pod

when we start to group together containers,

we are grouping together containers

that have a very discreet,

very tightly coupled relationship.

In other words, these are containers

that absolutely have a tight integration

and must be executed with each other.

So as an example,

let's imagine a pod that is running three containers

that you see right here.

This would be a good example of when we would use a pod

with multiple containers.

So in this example, I've got a Postgres container,

so that's running the Postgres database.

And then in addition to that we've got two other containers.

Now, these are imaginary images that I've listed here,

so there is no image called logger that I know of,

there probably is, but, yeah I'm just making this thing up

and I don't really know of any image called backup manager.

So again, these are two imaginary containers

that we're just kind of pretending exist.

You could pretend that the logger container

might need to connect over to Postgres

and store some database logs

and maybe this backup manager needs to connect

to the Postgres container and do a sequel dump

or something like that and make a backup of all the data

inside that database.

And so again, this would be a very good example

of why we would want to use a pod with multiple containers.

A logger like this right here

is a hundred percent intended to connect

to this Postgres container

and pull some information out of it.

If the Postgres container goes away,

the logger a hundred percent worthless.

No two ways about it, the logger is completely useless

at that point in time

and it's the same thing with the backup manager as well.

The backup manager

might need to reach into this Postgres container

and pull out some information to make a database backup.

And so the backup manager a hundred percent

has a very tight integration,

very close relationship with the Postgres container

and again, if that container goes away,

the backup manager no two ways about it,

a hundred percent worthless.

So again, in the world of Kubernetes,

we make use of pods as the smallest thing

that we can deploy.

We cannot deploy individual containers by themselves

as we could with Docker Compose or Elastic Beanstalk.

Anytime we want to deploy a container

in the world of Kubernetes, we have to make use of a pod.

A pod can run one or more containers inside of it.

Inside this course, you and I are only going to be running

one container inside of any given pod,

but reasons that you might want to add in more containers

is if you have some other containers

that have a very, very tight integration

with other containers that exist inside the pod.

So with all that in mind,

if we flip back over to our client pod dot yaml file,

again we're specifying a object type of pod inside of here

and hopefully now

some of the other configurations inside of here

might make a little bit more sense.

We are creating a pod that's going to run

one container inside of it.

We're going to give that container

an arbitrary name of client.

The name of client right here is for our purposes,

largely going to be related to logging

and giving us the ability to reference

this running container,

but if we were running other containers

inside of here as well, we could also use this name property

to get some networking or connections

between these different containers

that are running inside the single pod.

The image property, well, I get,

bet you can guess what that is.

That's the name of the image or the repository

off on Docker Hub that this container

is going to be made out of.

And then finally, the ports entry on here is, eh, you know,

it's kind of like that port, excuse me,

that port mapping stuff that we were doing before.

The ports of container port right here

is essentially saying that on this container

that we're going to create, we want to expose Port 3000

to the outside world.

And you might be curious,

why are we exposing Port 3000 in particular

to the outside world?

Well, if you recall, back to our multi-client project

here's the source code for multi-client right here.

There's the client directory.

We had created a NGINX server

that runs inside of that container

and it listens on Port 3000.

So we set up that container Port 3000 right here,

because we're going to eventually want to expose

Port 3000 to the outside world.

So this line, at least right here,

is very similar to some of the stuff

that we did back inside of our

docker run dot AWS dot JSON file with Elastic Beanstalk

and it's also similar to some of the stuff that we did

inside of our docker compose file as well.

This is setting up our port mapping.

Now, one thing I wanna be really really clear about

is that this is not the full story of port mapping

inside the world of Kubernetes.

This line of code right here alone,

is not going to give us access to Port 3000

inside of our running container.

In order to get that networking set up,

that was the entire job of the second configuration file

we put together

and so that's gonna serve as a nice segue.

Let's take a quick break right here,

when we come back, we're going to start talking about

the purpose of this service object

that we put together inside the second file as well.

So quick pause and I'll see you in just a minute.

client-pod.yaml
```
apiVersion: v1

kind: Pod

metadata:

name: client-pod

	labels:

		component: web

spec:

	containers:

		- name: client

		  image: stephengrider/multi-client

		  ports:

			- containerPort: 3000 --> this is setting up our port mapping to some extent. not the complete step for port mapping though. it's mainly the second client-node-port.yaml to setup the complete network port mapping.
```

