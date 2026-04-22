![[Screenshot 2026-04-20 at 12.39.03 PM.png]]
![[Screenshot 2026-04-20 at 12.40.08 PM.png]]
![[Screenshot 2026-04-20 at 12.41.13 PM.png]]
![[Screenshot 2026-04-20 at 12.44.15 PM.png]]
Narrator: In the last section,

we had a quick discussion around the differences

between Ingress-NGINX and Kubernetes ingress.

We're now going to continue and start to talk

about how this ingress stuff works

behind the scenes to somehow get some amount

of traffic into your application.

So, let's get to it.

Now, I wanna first begin by giving you a quick reminder

of how we've been doing things

throughout this entire course.

Throughout the entire course, we've been making config

files that contain the desired state of our application.

So for example, we've been writing config files that say

that we want to be running three pods or three replicas,

each of which run the multi-client image.

We then feed that into kubectl

and that creates a deployment object, and it's up

to that deployment object to look at our current state

and then look at our desired state and figure

out some migration path or some way to get

from the current state to the desired state.

And so after the deployment jumps

into effect and starts working,

it's going to eventually create three new pods

each of which are running the multi-client image.

Now, the reason I'm giving you this reminder

about how all this stuff works is

that this deployment object right here is what

we're referred to as a type of controller.

In Kubernetes, a controller is any type of object

that constantly works to make some desired state

a reality inside of our cluster.

So in the world of ingresses, and as we start to talk

about ingress, the exact same strategy right

here all applies a hundred percent.

You and I are going to write some config file

that describes a set of routing rules

to take incoming traffic and send it

off to the appropriate services inside of our cluster.

We're then going to feed that config file into kubectl

And kubectl is going to create something called

an ingress controller.

So it is a controller, it is something that's going to look

at our current state, it's gonna look at the desired state

and then create some infrastructure that's going to

make our desired state a reality.

So in the case of our particular ingress controller

which is, as you might guess, using NGINX behind the scenes,

when we feed in this config file, the controller is going

to create a pod running NGINX that's gonna have

a very particular set of rules to make sure

that traffic comes in and gets sent off

to the appropriate different services inside of our cluster.

So you can kind of think that this entire setup

around ingress looks a little something like this.

We have our ingress config over here,

which is our config file that describes

all the routing rules that we want

to have inside of our application.

That's going to be fed into kubectl,

where an ingress controller is going to

be constantly working behind the scenes to make sure

that all of the routing rules that we define

inside the ingress config are actually implemented and met.

And so it's going to be up to the ingress controller

to create something, something you know,

who knows what it is, but something

inside of our cluster that will take the incoming traffic,

read some parameters in that traffic,

and then send it off to the appropriate service.

So the big takeaway right here, the only thing

I want you to understand right now is that you

and I are gonna create something called an ingress config

which is going to be a set of routing rules.

We're going to feed it into kubectl

which is going to create this ingress controller.

And the ingresses controller's job is to look

at the ingress config or that set

of routing rules and make that a reality.

The ingress controller has to create some infrastructure

inside of our cluster to make sure

that we're actually obeying those routing rules.

And so the ingress controller is going to make, again,

some thing that accepts incoming traffic.

Okay, so that's the kind of high level description

of what's going on here.

Now, in the case of the project that we are using,

Ingress-NGINX, things are going to work very similar

to this but just a tiny, tiny, tiny bit differently.

And so infinitesimally differently, differently

that I almost don't even want to tell you

that it's working differently.

But I am anyways just so you understand

how things are working behind the scenes.

So with the very particular project that we are using

of Ingress-NGINX, the ingress controller

and the thing that actually accepts traffic and routes it

off to the appropriate location is actually the same thing.

So with the very particular ingress project

that we are using, the ingress controller

and the thing that routes traffic is the same thing.

So we can kind of imagine, again, that you

and I are going to make a ingress config

or a set of routing rules.

We're gonna feed it into kubectl

and then the project that we are using,

Ingress-NGINX, is gonna create a single deployment,

whose job is to both read in the ingress config

and simultaneously create a pod that meets all

of those different routing rules.

Now again, this is a very tiny, tiny, tiny distinction

and I only mention this to you because we are going to look

at some of the behind the scenes setup

of Ingress-NGINX inside of our project.

And you're going to notice that there are not

like the three separate things that I show you right here.

In our project, there's only the two separate things

as shown in this diagram right here.

Okay, so with that in mind, let's take a quick pause.

We're gonna come back to the next section

and there's just a little bit more

around some behind the scenes action

for this ingress stuff that I wanna show you

before we start going through the setup.