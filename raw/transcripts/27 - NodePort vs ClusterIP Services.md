![[Screenshot 2026-04-19 at 8.21.07 PM.png]]

-: In the last section

we put together our multi-client deployment config file.

Now, we had already written out all the configuration

for this before,

and it's essentially a clone of what we already had.

But again, I just wanted you to get used to some

of the pieces of terminology inside of here

because you're going to be writing quite a bit

of these configuration files over time.

All right, so now

that we've got the deployment put together,

we're going to start working on the cluster IP service.

You will recall that the previous deployment

we had worked on used a node port service.

So the first thing we're going to do

is figure out exactly what a cluster IP is.

All right, so first off,

I want you to recall that in the world of Kubernetes,

we use a service anytime that we want

to set up some networking for an object,

such as a single pod or a group of pods

that are managed by a deployment.

Now, we made use of a node port,

which is going to expose a single pod

or a cluster of pods to the outside world.

And that's why you were, and I were able to type in a IP,

or specifically the IP for our mini cube virtual machine,

and then a port in the 30,000 range.

It was completely the node port that allowed us

to access our running pod inside of our browser.

Now, the cluster IP is a little bit more restrictive form

of networking.

The cluster IP is going to allow any other object

inside of our cluster to access the object

that the cluster IP is pointing at,

but nobody from the outside world.

So in other words, people like you and me,

or anyone inside their web browser can access the object

that this service is married up to or pointing to.

So what does that mean practically?

Well, it means that when we assign a cluster IP

to any of these deployments that you see here,

anything else running inside of our cluster

can access whatever object the cluster IP is pointing at.

So in other words, this cluster IP right here

is going to provide access for everything else

inside of here to the Redis deployment.

It's what's going to allow the multi worker pod

to eventually connect to the copy of Redis

that is running gear.

If we did not have this cluster IP service,

then the Redis pod would be completely unreachable,

and nothing inside of our cluster would be allowed

to access that running pod.

So that's what the cluster IP is all about.

It provides access to an object, most commonly,

a set of pods, to everything else inside of our cluster.

Now, the one thing I want to be really clear about

is that a cluster IP is not like a node port,

in that it does not allow traffic to come in

from the outside world.

So this right here, this would not be allowed.

This does not work.

We cannot use a cluster IP to get access to a deployment

from outside of our cluster.

All right, so that's what a cluster IP is for.

In general, we're going to use cluster IPs anytime

that we have a service that is, or something, not a service,

but an object that is only supposed to be accessed

from people already inside of our cluster.

Now, the one thing that you will notice

is that of course we want the multi-client

and the multi-server to be accessible from our users,

but the way that is going to happen

is through the Ingress service.

Traffic is going to come into this Ingress,

and then once traffic has come in,

it is then effectively inside of our cluster.

And so from that point on,

our cluster IPs will be accessible only

through the Ingress service.

We'll talk more about the Ingress service later on

because obviously that's a totally different piece

of networking that we have to put together.

Okay, so that's pretty much it on cluster IP.

Let's take a quick pause right now.

When we come back,

in the next section,

we'll put together our config file to create a cluster IP

for our multi-client deployment.