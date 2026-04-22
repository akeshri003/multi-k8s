
![[Pasted image 20260417205856.png]]

In the last section, we went over the majority of our client-pod.yaml file.

We spoke about what a pod is and what its purpose is.

We also spoke about the spec section down here a little bit

but we have not yet spoken very much

about this metadata section.

Now, very quickly on metadata,

the name is pretty much what you would expect.

It's gonna name the pod that gets created

and this is mostly used for a lot of logging purposes.

We're gonna see the name of client-pod used

when we start using kubectl to print out information

about our running cluster at the terminal.

Now, the other piece of information inside

of metadata is labels right here with component web

and that's very tightly coupled to the other config file

that we're going to start to discuss right now.

So, in this section,

we're gonna focus on the second config file we put together,

the one with a kind of service.

So, let's talk about what's going on inside here.

All right, so at this point,

we've spoken about the object with type pod.

A pod is used anytime we want to run one

or more very closely related containers.

So, we're now gonna start talking

about a second kind of very commonly used object.

The second object type that we're gonna discuss,

and again, this is a type that we're going

to be discussing just so much throughout this course

because all this networking stuff

is so important to understand.

Anyways, second object type,

yeah, services.

We use this object type anytime that we want

to set up some amount of networking

inside of a Kubernetes cluster.

![[Screenshot 2026-04-17 at 9.00.48 PM.png]]


```yaml
apiVersion: v1

kind: Service

metadata:

	name: client-node-port

spec:

	type: NodePort --> specifying here the sub-type of service.

	ports:

		- ports: 3050

		targetPort: 3000

		nodePort: 31515

	selector:

		component: web

```

The purpose of a NodePort service is to expose a container to the outside world. (only good for dev purposes or to be able to allow you to access your web browser acess your container).

  
So, let's take a look at a couple diagrams

that are going to give you a better idea

of exactly what a service is and how it behaves.

And the first thing I want you to understand,

is that in the world of pods,

we have basically just pods

that's it, like that is the object type.

But in the world of services,

there are four very commonly used subtypes,

in fact there's only four subtypes.

And so I've listed all the subtypes

of the service object type right here.

So as a service,

we can create a service of type ClusterIP,

NodePort, Load balancer and Ingress.

Inside of our particular file,

we specified a object type

or a primary object type of service

with the kind property up here.

And then inside of the spec section,

we specified a subtype

or a type of service known as a NodePort.

So we are making a NodePort service.

The purpose of a NodePort service is to expose a container

to the outside world,

or in other words,

to be able to allow you,

like you as a developer on your computer,

to open up your web browser

and access that running container.

A NodePort service is only good for development purposes

and we do not use NodePort as a service type

inside of production environments,

outside of one or two very specific exceptions

and actually, one of those exceptions

is something we're going to cover later on,

inside this course.

Now, you'll notice that on the other service types on here,

I'm not putting any notes or anything on here,

we are going talk about the other service types

in tremendous detail throughout this course,

again, services and networking

is a very important topic to understand,

but if I added in those other descriptions right now,

it would just be a little bit confusing,

so I'm gonna leave the other descriptions off.

And for right now,

we're just gonna focus on the NodePort service type.

Okay, so again NodePort is something that we use

to expose a container to the outside world

and essentially allow you and I to access that container inside of our browser.

![[Screenshot 2026-04-17 at 9.05.25 PM.png]]

Now, I wanna show you a series of diagrams

to give you a better idea

of what this service is doing for us,

so inside this diagram,

the overall blue box is our computer.

And so at some point in time,

you and I are going to want

to open up our browser and connect

to that multi-client container

that is running inside of our pod.

That pod, and the container inside of it,

are running on the local Kubernetes node

Now, I wanna show you a series of diagrams

to give you a better idea

of what this service is doing for us,

so inside this diagram,

the overall blue box is our computer.

And so at some point in time,

you and I are going to want

to open up our browser and connect

to that multi-client container

that is running inside of our pod.

That pod, and the container inside of it,

are running on the local Kubernetes node.

Remember this is the,

VM,

created by minikube,

that is running on your local machine.

Now, when we create that service of type NodePort,

it's gonna set up a communication layer

between the outside world

and the container running inside of that pod.

You'll notice that there's another box

on here called the kube-proxy.

Every single node or every single member

of a Kubernetes cluster that we create

has a program on it called the kube-proxy.

The kube-proxy is essentially the one single window

to the outside world.

So anytime that request comes into a node,

it's going to flow through this thing called the kube-proxy.

This proxy is going to inspect the request

and decide how to route it

to different services or different pods

that we may have created inside of this node.

So right now, I'm only reflecting one service right here.

But over time, we might end up

with multiple different services.

Something like that right there,

as messy as that may be.

And so when we end up with multiple services

inside of a single node,

it would be up to kube-proxy

to make sure that incoming requests,

are sent off to the appropriate service.

Now, when request comes into the NodePort service

that you and I are going to create,

it's going to attempt to take that request

and forward it onto port 3000

on our multi-client container,

that we defined to run inside of our pod.

Now, that gives you an idea of the placement

of the service here,

but that doesn't really,

kind of describe everything else

that's going on inside of here.

So, what's going on with the three different ports in our client-node-port.yaml?

And what's going on with the selector down here?


![[Screenshot 2026-04-17 at 9.11.17 PM.png]]


The port section in the client-node-port.yaml talks about the collection of ports that need to be opened up or mapped to different ports on the target object.

Now taking about each of the individual properties in a particular port collection - 
![[Screenshot 2026-04-17 at 10.12.39 PM.png]]

**Port** - the port that another pod or container inside our application could access in order oto get access to the multi-client pod. 

**targetPort** - its the port inside the port, to which we want to open up any incoming traffic to this pod. 

**nodePort** - it is the one that you probably care about the most, its the port that we wanna specify in our browser url to access or target access a specific pod.

We do not make use of nodePort service in production. 

Remember this is the,

VM,

created by minikube,

that is running on your local machine.

Now, when we create that service of type NodePort,

it's gonna set up a communication layer

between the outside world

and the container running inside of that pod.

You'll notice that there's another box

on here called the kube-proxy.

Every single node or every single member

of a Kubernetes cluster that we create

has a program on it called the kube-proxy.

The kube-proxy is essentially the one single window

to the outside world.

So anytime that request comes into a node,

it's going to flow through this thing called the kube-proxy.

This proxy is going to inspect the request

and decide how to route it

to different services or different pods

that we may have created inside of this node.

So right now, I'm only reflecting one service right here.

But over time, we might end up

with multiple different services.

Something like that right there,

as messy as that may be.

And so when we end up with multiple services

inside of a single node,

it would be up to kube-proxy

to make sure that incoming requests,

are sent off to the appropriate service.

Now, when request comes into the NodePort service

that you and I are going to create,

it's going to attempt to take that request

and forward it onto port 3000

on our multi-client container,

that we defined to run inside of our pod.

Now, that gives you an idea of the placement

of the service here,

but that doesn't really,

kind of describe everything else

that's going on inside of here.

So, what's going on with the three different ports?

And what's going on with the selector down here?

Let's take a look at another diagram

that's going to be slightly more detailed

than this one right here,

that's gonna give you a better idea

of those different pieces of configuration

and their purpose.

Okay, so very similar diagram,

just with more detail added in.

I just gotta fix that thing or I'm gonna go crazy.

Okay, so on the service you'll notice I added in,

kind of a long statement right here.

If you look at the client's configuration file,

you'll notice that at the very bottom is a selector

that says component:web

You'll also notice that there's nothing else inside

this file that seems to indicate,

that this service should be handling network traffic

for our pod with a name of client-pod.

Here's the pod file,

it's got a name of client-pod.

Here's the service file,

nowhere in here do we say client-pod.

So in other words,

there's nothing inside of here

that says I need to send...

traffic to client-pod.

There's no declaration like that inside of here.

Instead, rather than referring to the service,

versus maybe the pod that we want to have

this service direct traffic to,

rather than using any naming system,

we instead use a system in Kubernetes

called the Label Selector System.

So inside of this service file,

you'll notice that down at the bottom

we have a selector of component:web

And then over here back inside of the pod file,

we have a metadata labels property of component:web

That's how these two different objects get linked together.

When the service first boots up,

it's going to say,

okay, I need to do some port forwarding.

I don't know who I'm supposed to forward this traffic to.

It's then going to see its selector property down here

and it's gonna say,

oh okay, I'm gonna look for any other pod

or any other object that is running

that has a key value pair,

of component:web

And if I see any other object running

inside this Kubernetes cluster

with the label of component:web

I'm going to direct all traffic

to these ports on that thing.

So the service that we've created

and the pod that we created are linked together

by the label inside the client-pod

and the selector inside the service.

Now, the one thing that I want to make sure

is really clear is that the selector

that we've used here of component:web

is 100% arbitrary.

So we could have just as easily called this thing like tier,

front-end.

If we had made that change,

we would just need to make sure

that the client pod has the same label as well.

And so back inside the client pod,

we would need to have tier front-end as well,

to make sure that the two are a hundred percent identical

and match up perfectly.

Now, I am going to undo those changes

and I'm gonna go back over to component:web

on both configuration files.

Okay, so again,

when the service boots up, it says

I'm going to look at my selector property

and it's going to see

that it has a key value pair of component:web

It's then going to reach out in the Kubernetes cluster

and it's gonna find every other object that has a label

of component:web

And it's going to attempt to expose

port 3000 to the outside world more or less.

And so of course our pod needs

to have the appropriate label of component:web

Okay so,

that brings us up to speed on

the selector inside the client-node pod file

and the label in the client-pod file.

Now, the last thing I wanna tell you about,

is the collection of ports right here.

We have ports,

TargetPort,

NodePort, all these different things.

Now, one quick thing I just realized,

I put ports right here,

that is a little typo

it should be port singular,

like so my mistake.

I'll mention that again at the end of this video just

to make sure that if you're speeding through this section,

you don't miss out on that little fact.

Okay, so in this service we have a port section.

This is describing all the different collections

of ports that need to be opened up

or mapped on the target object.

One thing I wanna point out here,

is that the service ports property is an array

and so we could very easily have additional ports

that we're trying to be mapped on here as well.

So we could do like, I don't know,

9,000 or whatever it might be.

Now, something that might seem a little bit weird is

the fact that we have three different ports inside of here.

First thing I want you to notice is

that the TargetPort of 3000 is identical

to the container port over inside of the pod definition.

Now, that's not really a lot of help,

let's just look at a diagram.

It's gonna explain these different things.

All right, I'm wasting time.

Here we go.

Okay so, inside the NodePort service,

we're exposing those three different properties

so we have defined those three different properties.

Now, the first thing

that we're defining inside there is port.

Port for you and me is more

or less a hundred percent worthless

for the application that we are putting together right now.

The port property is going to be the port that another pod

or another container inside of our application could access

in order to get access to the multi-client pod.

So this multi-client pod right here,

that represents the actual pod

that the service is trying to map traffic over to,

so we can imagine,

if there is some other pod inside of our application,

like let's say other pod,

right here,

there's some other pod that needs access to multi-client,

it could connect to it through

this port designation right here.

So again, for you and I,

for the application we're doing right now,

this port property is not useful

because we do not have any other objects

or anything else inside of our Kubernetes cluster

that's going to attempt to reach into that multi-client pod.

Now, the next property inside there is the TargetPort.

So as you might imagine,

the TargetPort is the port inside

of that pod that we want to open up traffic to.

We used a TargetPort of 3000 right here,

which indicates that we want

to send any incoming traffic into port 3000,

inside of this pod.

And port 3000 has been mapped up

to the multi-client container.

Now, the last thing on here is the NodePort.

You'll notice that for the NodePort,

we used a rather, large port number.

I used 31515.

Now, the NodePort is the one that you

and I probably care about the most

whenever we make use of a NodePort type,

the NodePort is the port that you

and I inside of our browser are going to access

whenever you and I like want to actually test out

the container running inside that pod.

So the NodePort right here is going

to be essentially what you and I type into our browser.

So as the URL,

you know, we'll do whatever the IP address is,

colon three...

what was it?

31515.

So we're gonna type in that port

into our browser in order to access the multi-client pod.

So again, we can kind of ignore the port property

for this application we're doing right now.

The NodePort is what gets exposed

to the outside world and the TargetPort

is what gets opened up inside of the targeted pod.

Now, the last thing I wanna mention is

that the NodePort is going to always

be a number between 30,000

and 32,767.

If you do not specify the NodePort,

so we are not required to specify it,

we could actually delete it if we wanted to.

If we do not specify that port one

will be randomly assigned to us,

it'll be between 30,000 and 32,767 as well.

Now, the reason,

or I should say one of the reasons

that you and I do not make use

of the NodePort service in a production environment,

is because of these funky port mappings.

Obviously when a user goes to something like google.com

they wanna go to google.com

they don't wanna go to google.com:351515 or whatever right?

Now again, that's just one reason of many

that we do not use a NodePort in a production environment,

outside of some very specific exceptions.

Okay, so I think that explains

just about everything inside of our client-pod file

and the client-node port file as well.

So now the last thing we have to do

is take these two configuration files

and we're gonna load them into our Kubernetes cluster

through the kubectl command line tool.

So, quick pause and we'll come back in the next section.