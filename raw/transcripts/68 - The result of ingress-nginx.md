![[Screenshot 2026-04-22 at 8.44.22 PM.png]]
Instructor: In the last section, we used Helm

to install Ingress-NGINX inside of our project.

Now that we've got that installed,

I want you to refresh the page.

So I'm gonna refresh this entire thing.

I'm gonna say yes, go ahead and reload,

and then I'm gonna flip over to the Workloads tab over here.

Remember, the workloads tab is where we're going

to eventually see a list of all of our deployments.

So we can now very easily see

that we have something here called the Ingress controller,

and our Ingress default backend.

Remember, the Ingress controller is the pod,

or in this case, the deployment that manages the pod,

that runs the actual controller

that's going to read our Ingress config file

and then set up NGINX appropriately.

The default backend is, as it says, a default backend

that has a series of health checks inside of it.

If we were doing things really, really well,

we would set up a certain number of routes

inside of our Express API

to associate the health of our cluster with our actual API

as opposed to this default backend,

but in this case, using the default backend, totally fine.

Now, the other thing I wanna show you really quickly

is the Services tab over here.

Now, if you see something here that says Load Balancer

and you see these two sets

of numbers next to it, that's great.

If you do not see two sets of numbers right here,

then pause the video right now for, like, a couple minutes

and try refreshing the page again in a couple minutes.

After a little bit, you'll eventually see two sets

of numbers appear right here.

These two sets of numbers that you see

are the actual IP addresses

that we're going to eventually use to access our project.

And so right now, as a matter of fact,

you can click on the colon 80 address,

or the colon 80 port right there,

and it'll open up a new tab at that IP address,

and you'll see default backend 404 appear on the screen.

This message right here is being created by,

as you might guess, the default backend

that was created for us.

That's the purpose of the default backend.

Not only does it provide a health check,

it also provides a default 404 page

that our users will go to

if some route cannot be found inside of our application.

AlL right, so eventually we're going

to use this IP address right here

to get access to our application later on.

Now, before we continue,

there's one other quick thing I wanna show you.

If you go to the Navigation menu up here,

we'll then scroll down a little bit,

and inside of here, I always forget exactly where it is,

ah, here we go.

Network Services under the Networking section.

So, we're gonna go to Network Services

and they'll dump us out on the Load Balancing tab.

This right here is the Google Cloud load balancer

that was created for our cluster.

You can take a look at the load balancer details.

Essentially what it's saying right here

is that the load balancer can be accessed

at this IP address.

This is the same IP we just went to a second ago,

and the load balancer is governing access

to this set of different instances.

The set of instances right here that you see

are different virtual machines.

They represent the three nodes

that belong to our Kubernetes cluster.

So recall, we looked at this diagram a long time ago

when we were talking about Ingress-NGINX and Google Cloud.

We had said that when we use Ingress-NGINX,

it's going to create a Google Cloud load balancer.

So, that's what you're looking at right here.

This is the Google Cloud load balancer

that is not at all native to Kubernetes.

That load balancer is going to get traffic into our node,

or the entire cluster in this case,

and it's gonna point traffic to the Load Balancer Service,

which is a kubernetes service.

It's a type of service,

and that service is directing traffic

to our NGINX Ingress controller,

which is eventually gonna send traffic off

to all of our different deployments.

All right, so it's kind fun

to see this stuff all finally come together.

Now, the very last thing we have to do

is actually deploy our application.