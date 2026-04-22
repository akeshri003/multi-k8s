![[Screenshot 2026-04-20 at 12.29.09 PM.png]]
![[Screenshot 2026-04-20 at 12.30.29 PM.png]]

-: All right my friends,

we're almost done with doing our local setup of Kubernetes.

We've set up our deployments,

we've got the cluster IPs,

our persistent volumes, secrets, environment variables,

the whole shebang.

The very last thing we have to do,

is somehow allow traffic to get into our application.

Now, you will recall that we have previously allowed

people to connect directly

to the multi-client pod over here,

through the use of a node port.

But a node port is only really good for use

inside of a development environment,

and we certainly want to eventually

take this application into production.

So, rather than using a node port,

we're gonna start to wire up an ingress service

to our application.

Now, in this section in particular,

we're gonna get a couple of words in

on exactly what in ingress service is,

but before we do,

I want to tell you about the third type of service

that we had very quickly mentioned exists,

but didn't really go into a lot of detail on,

and that's the service called a load-balancer.

So, we're gonna very quickly give you a brief overview

on a load-balancer type service,

and talk about why we're not gonna use it,

and then we'll move on to talking about ingresses

and all that whatnot.

All right, so for services, remember,

these are objects that are meant to set up

some type of networking inside of our cluster.

We've made use of a cluster IP,

which allows other pods

to get access to a particular service,

or a particular set of pods.

We've got the node port,

which we just discussed again,

and the load-balancer.

Now, this is a legacy way

of getting some amount of traffic into your application.

You're going to see a lot of documents, or blog posts,

referred to load-balancers,

but whenever you do,

check the date on the blog post,

it might be a little bit dated.

An ingress is the newer, and supposedly better way

of getting traffic into your cluster.

But again, quick discussion on a load-balancer,

and why we're not gonna use it.

All right, so a load-balancer actually does

two separate things inside of your cluster.

First off, to create a load-balancer type service,

we would make a configuration file of type service,

and a sub-type of load-balancer.

When you set that up,

you're going to essentially allow access

to one specific set of pods inside of your application.

Remember, the goal of a service is to kind of manage access,

or provide networking to reach a set of pods.

And so, a load-balancer only is going to give you access

to one set of pods.

In our case, our application has two set of pods

that we want to expose to the outside world,

which is 100% common.

Your application might need to have

a very similar type of setup.

So, a load-balancer would not be able to give us access

to both the multi-server set

and the multi-client as well.

Now, when you make a load-balancer service,

you are not only getting access to a specific set of pods.

When you make a load-balancer service,

Kubernetes is going to actually

do something in the background for you, as well.

It's going to reach out to your cloud provider,

so be it AWS, or Google Cloud,

or whoever else you might be using,

and it's going to create a load-balancer

using their configuration,

or definition of what a load-balancer is.

So, in the AWS world,

you might get a classic load-balancer,

or an application load-balancer.

There's one of their third kind of load-balancer

that escapes me off the top of my head, as well.

Kubernetes is going to tell AWS,

"Hey, I need like a application load-balancer,

or a classic load-balancer."

So, it's going to set up this external resource

outside of your cluster,

and then it's going to automatically configure

that load-balancer to send traffic into your cluster

and access the load-balancer service

that has been set up to govern access

to this very specific set of pods.

So, that's what a load-balancer does.

Again, kind of a more legacy thing.

Supposedly, some people say it's deprecated,

but it's really hard to look at

the Kubernetes official documentation,

and see something that specifically says deprecated.

So, I'm not 100% sure if it is deprecated,

but, certainly older way,

and not really appropriate for us, because again,

we've got these two different things

that we want to expose to the outside world.

So, with that in mind,

we are going to be using an ingress service instead

to get traffic into our cluster.

So, let's take a quick pause right here,

in the next section,

I want to give you a couple quick notes on ingress,

some high-level things that you just need to be aware of.