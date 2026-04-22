![[Screenshot 2026-04-20 at 12.46.11 PM.png]]
![[Screenshot 2026-04-20 at 12.54.06 PM.png]]

Instructor: In the last section, we spoke

about the three separate pieces

of setting up Ingress inside of Kubernetes.

So we've got the Ingress config,

we've got the thing that actually implements

that configuration, and then we've got the thing

that actually takes incoming traffic

and sends it off to the appropriate service.

So I now want to take this type of approach right here

and apply it to the actual application

that we are looking at.

Now, there is gonna be one little twist here.

Remember that I had told you just a little bit ago

that the setup of all this Ingress stuff

changes a little bit depending upon your environment.

So we're gonna look at a diagram here

that describes how all this stuff is set up

specifically on Google Cloud.

So the actual implementation when we set this up locally

is gonna be just a little bit different,

but the Google Cloud implementation

is probably the easiest to understand

and kinda wrap your head around.

So here's what's gonna happen.

Again, you and I are going to make an in Ingress config file

and that's going to describe all the routing rules

that should exist inside of our application.

This config is going to be fed into a deployment

that is running both our controller,

the Ingress controller, and an NGINX pod.

And that NGINX pod

is what's going to actually take incoming traffic

and route it off to the appropriate location.

Now, specifically on Google Cloud,

there's some other pieces in here

that get created at the same time as well.

So when we make an Ingress on Google Cloud,

we are going to get a Google Cloud load balancer

created for us.

This is a cloud native load balancer,

identical to the one that you would use

with any other type of application

that you might happen to create on Google Cloud.

In fact, you can open up a new tab right now

and search Google Cloud Load Balancer

and you'll see some marketing page here

for Google Cloud boad balancing.

So this is the actual object behind the scenes

that's going to be created by Google Cloud

to somehow get traffic into our cluster.

Once that load balancer is in effect

inside the cluster itself,

the deployment that gets created

is going to get a load balancer service attached to it.

Yes, the same load balancer service

that we had discussed a little bit ago.

So even though we had spoken about this thing

and we had said oh yeah,

load balancer service is kind of a little bit legacy

and we don't really use it anymore

and we prefer to use Ingress.

Well, behind the scenes on Google Cloud,

the load balancer service is still being used

and you wouldn't quite know it

unless you actually looked at some of the source code

or config files that set all this stuff up.

So incoming traffic will come

to the Google Cloud Load Balancer.

That load balancer is going to send that traffic

to a load balancer service inside the cluster,

which is going to eventually get that traffic

into the NGINX pod that gets created

by our Ingress controller.

It's then up to that NGINX pod

to eventually send that traffic off

to the appropriate service inside of our application,

either the multi-client or the multi-server.

Now there's one last little piece of the puzzle here

that I just want you to be aware of.

By default when you set up all this NGINX and Ingress stuff

there's going to be another deployment

set up inside of your cluster,

something called a default backend.

The default backend is used for a series of health checks

to essentially make sure

that your cluster is working the way

that it should be working.

Now we're going to be making use

of the default backend inside this course,

but ideally, like in an ideal world,

you would actually replace

the default backend pod right here

with your Express API server.

And so anytime that the to next pod

needs to check the health status of our application,

it would be ideal to tell it to reach out

to the multi-server as opposed to the default backend.

And I'll show you very quickly

how you could do that later on.

But for right now, I just want you to be aware

that there is going to be this additional deployment

inside of our application.

Okay, So that's it.

That's what's happening behind the scenes.

Now, one last quick thing here

something that you might be a little bit curious about.

So at this point we've said oh yeah,

you know, we're actually using this load balancer service

and the load balancer service is getting traffic

eventually into an NGINX pod.

So why, oh why would we not just do this stuff manually

to a degree?

You know, why are we gonna go through all this extra setup

of this Ingress stuff and the Ingress config

and a whole bunch of additional setup on top of that

when we could take a much more straightforward

and simplistic approach?

Something like this right here.

Why didn't we not just set up

our own load balancer service manually

that points to our own custom copy of NGINX?

We've already made use of NGINX quite a bit

throughout this course,

so why do you and I not just make our own

kind of custom NGINX server that sends traffic off

to the appropriate set of pods?

Well, the reason that we are still going to make use

of Ingress NGINX as opposed

to setting this stuff up manually,

is that when you make use of the NGINX Ingress project,

it actually has a bunch of code added into it

that is very much aware of the fact

that it's operating inside of a Kubernetes cluster.

So as one little example, there's many examples of this,

but I'm gonna give you one very quick example.

When you make use of the NGINX Ingress project,

rather than taking incoming traffic

and sending it off to the cluster IP

and allowing the cluster IP to load balance that request

off to some random pod inside of here,

the NGINX Ingress project is not actually gonna send traffic

over to the cluster IP service

and allow that to do some amount of load balancing.

Instead, the cluster IP service does still exist

and it still is working to essentially keep track

of all these different pods.

But anytime we get a incoming request

to the NGINX pod right here, the NGINX pod

is going to actually route the request

directly to one of these different pods,

completely bypassing that cluster IP service.

The reason that this is done

is to allow for features like say sticky sessions,

which is a reference to the fact

that we sometimes want to make sure

that if one user sends two requests to our application,

we want to make sure that both those requests

go to the exact same server.

And there's some cases where that feature is extremely handy

and actually a hundred percent necessary to have.

So that's just one example of why we are making use

of this Ingress NGINX project

as opposed to kind of doing our own

build it yourself solution.

So you certainly can do a build it yourself solution

it's just going to lose out

on a couple of these extra features that you get for free

when you make use of the Ingress NGINX project.