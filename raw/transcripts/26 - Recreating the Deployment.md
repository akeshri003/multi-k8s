-: In the last section, we tested out

our multi-container application with Docker Compose,

just to make sure that everything was still working

the way we expect.

I'm now going to close down Docker Compose

and return back to the Command line.

Now from this section on, we're gonna start to make changes

to our project inside the complex directory,

so if you want to keep a record

of all the stuff that we had done for Elastic Beanstalk

and for Docker Compose and whatnot,

feel free to make a backup of this folder right now.

I, in particular, already made a copy of this folder.

I made a copy and called it Complex Elastic Beanstalk,

just to serve as a backup of sorts.

Now, of course, I can rely upon Git as a backup as well.

Just to make it easy for you to refer to all of my code

at some point in the future,

I made the easy backup folder right here.

All right, so I'm gonna go back into the complex directory

and then I'm going to start up my code editor

inside this folder.

Now we're going to first begin

by cleaning up our project directory a little bit.

We're going to delete some of the different files

and folders that are not going to be required

for this new Kubernetes version of this application.

So the first thing I'm going to do

is find the travis.yml file, the Docker Compose

and the Docker run files,

and I'm going to delete all three of those.

We do not need these files anymore

because we're going to rely upon Kubernetes

to run our application both in a development

and production environment,

and for the travis.yml file,

we're going to essentially recreate that thing from scratch

when we eventually try to take this application

to Travis again.

Now, I'm also going to delete the NGINX folder.

We previously had a NGINX image

to serve as kind of the primary routing

inside of our entire cluster of containers,

but now for routing, we're going to rely upon something

called the Ingress service, which we're going to talk about

at great length in a little bit,

but for right now, just know that we don't need

this NGINX image anymore,

so I'm gonna delete that thing as well.

All right, so now I have just client, server and worker,

and nothing else.

Now, the first thing I'm going to do

is create a new folder inside of here called K8s, like so,

and we're going to use this folder

to house all of the different configuration files

that we're going to create for our project.

In total, we're going to have

one separate configuration file

for each of these different objects

that you see on the screen.

So 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11,

11 configuration files.

Yeah, that's a lot, but hey, we'll get through it.

So let's first begin by putting together

a configuration file to create our multi-client deployment.

We've already put together a configuration file for this

on the project we were just working on a second ago

as we were first learning Kubernetes,

but we're just gonna recreate the thing

from scratch right now,

just to get some of the common typing

that we have to do inside of our head.

All right, so inside of my K8s folder,

I'm going to make a new file called client-deployment.yml.

And then inside of here, we'll put together

all the configuration required to create this deployment

with three child pods running the multi-client image.

So at the very top, we'll start off by designating

the API version that we want to use,

which is going to be Apps v1.

We'll specify the kind of object that we're going to create

inside of this file, it's going to be a deployment.

And then we'll add on some metadata,

in this case just the name of the deployment,

and it's going to be Our Client Deployment.

Next up, we'll add a spec

that's going to configure this deployment.

So we're going to say that we want three replicas

of the multi-client image,

or really the multi-client pod, running,

and we want those three replicas to be managed

by this deployment.

After that, we'll put down our selector,

I'll say match labels,

and then I'm going to use the same key value pair

as a label that we had used previously,

which is to say component web.

In other words, this means that the component

of our application that this deployment

is going to be managing is the web

or front end side of things.

After that, we'll specify the pod template.

Now remember, the template right here is going to be

on the same indentation layer as the selector above it.

So for the template, I'll provide some metadata

that will provide a label of component web.

Remember, the selector out here and the label

inside the template is how the deployment

is going to identify the different pods

that it's supposed to manage.

Next up, we're going to add a spec, and again,

this is going to be on the same indentation level

as metadata.

And then we'll specify all the different containers

that are going to run inside this pod.

In this case, we have just one container.

It's gonna have a name of Client.

The image we want it to use will be Your Docker id,

multi-client.

And then finally we'll specify a port to open.

We'll use a container port of 3000,

and that's gonna be the port that is mapped up

to the multi-client image.

All right, so that's pretty much it

for our client deployment.

Let's take a quick pause right now,

when we come back in the next section,

we're going to very quickly put together

a new configuration file for this service right here,

and we'll talk a little bit

about exactly what a cluster IP is,

because previously we had only used a node port.

So, quick break, and I'll see you in just a minute.

```client-deployment.yaml
apiVersion: apps/v1

kind: Deployment

metadata:

	name: client-deployment

spec:

	replicas: 3

	selector:

		matchLabels:

			component: web

	template:

		metadata:

			label:

				component: web

		spec:

			containers:

				- name: client

				  image: stephengrider/multi-client

				  ports:

					- containerPort:3000
```


![[Pasted image 20260419201828.png]]