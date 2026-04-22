![[Screenshot 2026-04-19 at 6.24.15 PM.png]]![[Screenshot 2026-04-19 at 6.29.03 PM.png]]
-: At this point, I think that we understand some

of the core components of the world of Kubernetes.

We understand what pods are, what deployments are,

and how we can expose pods

to the outside world through the use of basic services

like the cluster IP service.

In this section,

we're going to start taking our previous application,

so that multi container app that was calculating

in Fibonacci values, and we're going to

bring it into the world of Kubernetes.

We've kind of already been going

down that path with this simple K8's folder, right?

We've been setting up our multi client image a little bit,

and working around with it,

but we're not gonna start putting a concerted effort

together to take that previous multi container

application we had put together,

and adopt it into a sort of Kubernetes architecture.

I've got a diagram that's gonna be a high level overview

of the general architecture that we're going to use.

So, everything that you see here is in a single node.

We're going to first develop all this stuff locally.

We're gonna set up the entire application

on our local Kubernetes cluster.

After we get this all put together locally,

we're then going to deploy it off

to a service provider such as AWS or Google Cloud.

When we deploy it out there,

we will have the option to expand to multiple nodes.

So, we won't necessarily just be limited

to one single virtual machine

for all these different objects that we're going to create.

But for right now, we're just going to kind of imagine

that it's all sitting on one single note.

Now, this diagram is showing the general overall

architecture that we're going to use

for us moving our multi container app

into the world of Kubernetes.

You'll notice that there are some

very familiar pieces in here.

So, we've got the multi-client set of pods

all being managed by deployment.

We've got the multi-server, remember, that's the express

API being managed by deployment.

And then you'll notice that I've also added

in Redis as a pod this time around,

and Postgres as a pod this time around as well.

So, back on the multi container deployment

on Amazon Web services, when we used Elastic Beanstalk,

we delegated to these outside services provided by AWS

to manage our Redis and Postgres needs.

But now that we're moving over to Kubernetes,

we're going to put these things together

in a production environment ourselves, rather than relying

upon some outside service to do it for us.

The overall purpose of the app is going to stay the same.

So, it's still going to be all

about calculating some Fibonacci values.

I know it's kind of a silly example,

but it works, and it allows us to have a couple

of different services that work together in a nice fashion.

Now, you will notice that inside this diagram,

there are a couple of new terms as well.

So for example, on the far left hand side is something

called an ingress service.

You'll notice that rather than having the node ports

on our deployment for multi client that we had previously,

we now have a cluster IP.

And it looks

like just about every deployment that we put together,

with the exception of the multi worker right here,

has a cluster IP, kind is somewhat attached to it.

So of course, we're going to talk

about what an ingress is, and what a cluster IP is as well.

The other new piece of terminology that you'll notice inside

of here is something called a Postgres PVC.

PVC stands for Persistent Volume Claim, and of course,

we'll talk about what that means when we get

to setting up our Postgres stuff as well.

All right, so that's the idea.

We're going to take our previous application,

and we're going to move it into the world of Kubernetes.

Now, here's the general series of steps

that we're going to take.

So, in a development environment,

we're going to first create a config file

for every last thing that you just saw in that diagram.

So, we're gonna have configuration files

for the ingress service, for this cluster IP,

for this cluster IP, for this one, for this one,

and we're going to have config files

for every single one of these deployments as well.

And we'll also have a config file

for this persistent volume claim

down here too.

So in total, we're talking about like 10 or 12,

or something like that, different config files.

It is going to be a lot of typing,

but we definitely are going to learn a lot

about these configuration files along the way.

Now, once we put together all these config files,

we're then going to test everything out

locally on mini queue.

So, we're going to make sure

that everything works locally first,

and we're going to use it as our development environment.

We're going to be, as you know, kind of as confident

as we possibly can, that when we eventually push this

up to a production environment,

everything will work the way we expect.

After that, we're going to build a GitHub

and Travis deployment flow, very similar

to the one we had done previously

for both the single container

and multi container applications.

But of course, there will be some edits to it,

to make sure that everything is

set up for a Kubernetes deployment,

as opposed to the rather easy

and straightforward Elastic Beanstalk we were doing before.

And then of course, the very last step is to make sure

that we can actually deploy this entire thing

to a cloud provider like AWS or Google Cloud.

And yes, we're gonna go through that

entire series of steps together,

and make sure that people can actually visit our application

in a meaningful way.

All right, so we definitely have our work

cut out for us, no two ways about it.

So when we come back together in the next section,

we're going to flip back over

to our complex multi container project,

and we're going to start setting up config files

for everything inside of here.

So, quick break, and I'll see you in just a minute.