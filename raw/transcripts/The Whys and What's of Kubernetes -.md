
![[Pasted image 20260417182116.png]]
So what you see on the screen right here is a diagram

of a very simple Kubernetes cluster.

A cluster in the world of Kubernetes is the assembly

of something called a master, and one or more nodes.

A node, which is each of these blue boxes right here,

are a virtual machine or a physical computer

that is going to be used to run some number

of different containers.

So each of these different virtual machines

or physical computers that you see right here

can be used to run different sets of containers.

I wanna be really clear that each of them

can run different containers.

Like different images or even different numbers.

So for example, the node on the far right hand side

over here is running three separate containers,

and the one in the middle is running just one container.

These can be completely different containers.

They don't have to be identical.

And so in the world of Kubernetes, you can imagine

that we could have very easily gotten closer to a kind

of scaling flow like this right here.

Or maybe we had just one node or one virtual machine

that was running Nginx, our server in our client

and then maybe we had a bunch of additional nodes

or virtual machines that were running

copies of our worker image.

Now, in the world of Kubernetes

all these different nodes that get created are managed

by something called a master.

This master down here has a set

of different programs running on it that control what each

of these different nodes is running at any given time.

You and I as developers interact with a Kubernetes cluster

by reaching out to this master right here.

You and I give some set of directions to the master.

For example, we might say,

"Hey please run five containers using

the client worker image."

The master receives that command

and then ultimately relays that command

to all of these different nodes.

Now, outside of our cluster, which is represented

by this gray block box right here, we have a load balancer

which will take some amount of outside traffic

in the form of network requests and relay all those requests

into each of our different nodes.

Now, one thing that's going to be a big point of discussion

for us is exactly how this node balancer works

and so that's gonna be something we talk

about inside of the next many videos

on Kubernetes that we're going to go through as well.

Okay, so in this section, just a very high level discussion

on Kubernetes and what it is and what it does for us.

So at this point, I think we've kind of established

that Kubernetes is a system

for running many different containers, so different types

of containers, different numbers of containers

over several different computers or virtual machines.

And we would choose to use Kubernetes

if we had the need to scale up our application

and run multiple different types

of containers in different quantities.

If we only wanted to have a system where we essentially ran-

Let's go back way over here.

If we ran the same set

of containers over and over again, like you see right here,

not a lot of good reason to use Kubernetes.

Again, we want to use it anytime we expect to have

an application that consists

of many different types of containers,

running in different quantities

on multiple different computers.

All right, so with all that in mind

let's take a quick pause right here

and continue in the next section.
