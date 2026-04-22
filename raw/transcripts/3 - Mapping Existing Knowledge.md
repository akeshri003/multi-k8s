##### Mini Goal - 
Get **multi-client** image running on our local kubernetes cluster running as a container.

![[Screenshot 2026-04-17 at 6.38.24 PM.png]]
![[Screenshot 2026-04-17 at 6.40.58 PM.png]]![[Screenshot 2026-04-17 at 6.44.26 PM.png]]

-: In the last couple of sections,

we went through the setup process on Kubernetes

on Mac OS and Windows.

To make sure that everything is working as we expect now,

I'm gonna open up my terminal and run Minikube status.

When I do so, I should see Minikube running,

cluster running, and kubectl correctly configure.

I'm also going to run the Command kubectl cluster-info,

like so, and I should see a message

like this right here appear.

If you see any warning or any error message,

it means that something went wrong during the setup process

and you will want to flip over to the QA section

on this video and go through a little bit

of troubleshooting steps to get everything fixed up.

Okay, so now that everything is up and running,

we're gonna set ourselves a little short term goal.

So we're gonna say that as a short term goal,

we want to take the multi-client image

that we had put together over the last couple of sections,

and get it up and running on our Kubernetes cluster

that is now locally created on your machine

by Minikube running as a container.

So that's the goal.

Now, to just say this as a goal

without giving any kind of directions or path to get to it,

is kind of hard.

So I'm gonna show you a sequence of diagrams

that are going to give you a good idea

of how to transfer some of your existing knowledge

in the area of Docker Compose

over to all this Kubernetes stuff.

All right, so here we go.

We've put together several Docker Compose files

throughout this course,

and when we were working on the multi container project,

we ended up with a Docker file

that had a couple of different services inside of it.

And there's a couple of observations I want to make

about the Docker Compose files that we've created

throughout this course.

The first observation I want to make

is that in these Docker Compose files,

every entry, or in other words,

each of these different services that we created

can optionally use Docker Compose to build the image.

And we did that with Nginx, with worker,

and the client member.

Remember, we specified the build section

that provided the path to a context

and a Docker file as well.

The next observation I have

is that each of the entries inside of here

represents specifically a container,

a container we wanted to create.

We were only creating containers through Docker Compose.

We did not create any other type of software or program

or anything like that.

It was specifically containers that were being created

inside of our Docker Compose file.

Now, that might sound like a weird thing to observe,

but let's just hold on for that for right now.

The last observation I have is that every entry

inside the Docker Compose file

also optionally defined some networking requirements.

In other words, it set up all that port mapping, right.

It set up an outside port on our local machine

to a port inside one of our containers,

as was the case for say the Nginx image.

Okay, so we got these three observations right here.

Now I want to take these observations

and kind of map their equivalence into the Kubernetes world.

And so this is going to allow you to translate

some of your existing Docker Compose knowledge

into the Kubernetes world.

Okay, here we go.

So first off, each entry

that we put inside of that Docker Compose file

optionally allowed us to build an image.

So in the Kubernetes world,

we don't get any benefit like that.

With Kubernetes, we are expected to come

with all of our images already built.

So Kubernetes has no build pipeline.

There is no build process.

If you're gonna make use of Kubernetes,

the expectation is that during some outside step,

you're going to build all of your different images

and have them ready to go, ready for deployment

onto your Kubernetes cluster.

Now, the next big observation

was that each entry inside of our Docker Compose file

represented a container that we wanted to create.

With Kubernetes, things are just a little bit different.

With Kubernetes, we do not have a single config file.

Instead, we're going to end up

with multiple configuration files.

Each of these different configuration files

that we're going to put together

are going to attempt to create a different object.

An object is not necessarily going to be a container.

Now, you might be wondering well,

what would we ever create

when we're talking about these container things

than a container?

Well, we'll take a look at what it means

to create an object in just a second.

But for right now, just understand

that we use multiple configuration files

to create different objects that are going to be in use

inside of our Kubernetes application.

And the final thing I wanna mention,

so we had said that each entry

inside of the Docker Compose file

defined the different networking requirements.

So in the Kubernetes world,

we have to manually set up a vast majority

of all of our networking.

So remember in Docker Compose

if we created all of these different containers

inside of a single Docker Compose file,

we could very easily connect to other containers.

In addition, if we wanted to do any port mapping,

it was as easy as adding in a little entry

to the Docker Compose file.

So in the Kubernetes world,

the process of kind of joining together two containers

with networking or exposing a port on a container

to the outside world is a much more,

far more involved process.

And a vast majority of the work that we do

throughout the rest of this course

is going to be all focused a hundred percent

on some of these networking topics.

That's gonna be one of the biggest things that we discuss.

Okay, so with all these kind of mappings over in mind,

I now wanna think about how we're going

to essentially treat each of these items

as we think about our current goal

of getting the multi-client image working on Kubernetes.

All right, so I put a final column into this diagram

and these are going to be the steps

that you and I are going to go through

to make sure that we are able to get our simple container

up and running.

So when we think about Kubernetes expecting all images

to already be built,

that means that you and I are going to have to make sure

that our image, specifically the multi-client image

is already built and pushed up to Docker hub.

So we're just gonna confirm that that is the case.

And you'll recall that

through the last application that we worked on

we spent a tremendous amount of time making sure

that our images were being pushed up to Docker hub.

So our images should already be there.

We're just gonna make sure that that is the case.

Next up with Kubernetes

we make one config file per object we want to create.

So that means that we're definitely going to make

one config file to create our container.

Now, we're gonna go into great detail

on this right here, because technically we're not going

to be making our container per se.

We're going to making something else slightly different.

But again, we'll go into that in great detail.

Now finally, with Kubernetes

we have to set up all that networking manually.

So you and I are going

to make an additional configuration file

to set up some networking

between our container and essentially the outside world,

or in other words, remember with our multi-line image

we were starting up an Nginx server

and serving up some production React files.

And so if we want to be able to access that container

from within our web browser,

we're going to have to do a little bit

of networking setup with Kubernetes.

So in total,

we need to make sure our image is on Docker hub.

We need to make one config file to create our container

or something that's going to essentially represent

or contain the container.

Again, we'll talk about that in great detail.

And then we'll make a second config file

to set up some networking and make sure

that our container created during this step

is accessible from our web browser.

So quick pause, we're gonna come back to the next section

and we'll get started on step number one.

So I'll see you in just a minute.