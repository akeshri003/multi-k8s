![[Screenshot 2026-04-17 at 11.07.03 PM.png]]

-: In the last section, we went through

a very long-winded demonstration

of exactly what was happening inside of our local node.

Now, at this point in time, our understanding

of the Kubernetes architecture is kind of limited

to this diagram right here,

and a couple of discussions about, yeah,

some networking stuff and nodes and blah, blah, blah.

Very simple, straightforward stuff.

So in this section, I wanna change that in a big way.

We're gonna walk through the process

of exactly what happened when we fed that configuration file

into kubectl and get a better understanding

of what is kind of happening behind the scenes.

Now, before I do, I wanna give you

a very quick demonstration at my command line,

and I wanna show you something rather interesting, okay?

So I'm gonna flip over to my command line.

Now, some of these commands,

you are not going to be able to issue successfully.

If you run the commands I'm about to run,

you will see an error message.

So please don't try to run these.

Just follow along visually for a second.

I will show you how you can run these commands

in just a moment as well.

But for right now, just watch.

Okay, so I'm gonna first start off

by doing a kubectl get pods.

That's going to list out the current status

of our running pod right here.

And as you can see, it currently has restarts of zero

and an age of one hour.

Now, I wanna do something very interesting.

I'm going to execute a command.

You're gonna see a docker PS right here,

and you're gonna see that I'm running

a tremendous number of containers right now.

One of the containers I am running

is the multi-client image.

So this is the container that is running

inside that pod right now.

Now, I'm gonna take that container ID.

Again, this is a container that's running inside that pod.

I'm gonna take the ID,

and I'll do a docker kill with that ID.

So this is going to completely kill

that running container inside of that pod.

So that's it.

It is done, it's gone.

Now, I'm gonna do a docker PS again.

If I scroll all the way back up,

you're gonna very quickly see that it looks like,

hey, wait a minute, it's still there.

There is Steven Grider multi-client.

That is the container running inside that pod.

But you'll notice that the thing

was only created two seconds ago.

If I also run the command "kubectl get pods,"

you'll notice that there is now one restart

reflected inside of here as well.

And so the thing that I wanted to demo to here very quickly

that's going to kinda play into the diagram

that we're gonna look at in just a second

is that it appears that if we manually delete,

or if one those containers inside that pod crashes,

it appears that it automatically

gets restarted for us for free.

So with that in mind, we're not gonna look

at a rather complex diagram.

We're gonna walk through the process of what happened

when we deployed or applied, I should say,

these two configuration files.

Now, just a quick disclaimer, this is going to be

a rather long video as we go through this discussion.


![[Screenshot 2026-04-17 at 11.10.28 PM.png]]
![[Screenshot 2026-04-17 at 11.20.18 PM.png]]

-: In the last section, we went through

a very long-winded demonstration

of exactly what was happening inside of our local node.

Now, at this point in time, our understanding

of the Kubernetes architecture is kind of limited

to this diagram right here,

and a couple of discussions about, yeah,

some networking stuff and nodes and blah, blah, blah.

Very simple, straightforward stuff.

So in this section, I wanna change that in a big way.

We're gonna walk through the process

of exactly what happened when we fed that configuration file

into kubectl and get a better understanding

of what is kind of happening behind the scenes.

Now, before I do, I wanna give you

a very quick demonstration at my command line,

and I wanna show you something rather interesting, okay?

So I'm gonna flip over to my command line.

Now, some of these commands,

you are not going to be able to issue successfully.

If you run the commands I'm about to run,

you will see an error message.

So please don't try to run these.

Just follow along visually for a second.

I will show you how you can run these commands

in just a moment as well.

But for right now, just watch.

Okay, so I'm gonna first start off

by doing a kubectl get pods.

That's going to list out the current status

of our running pod right here.

And as you can see, it currently has restarts of zero

and an age of one hour.

Now, I wanna do something very interesting.

I'm going to execute a command.

You're gonna see a docker PS right here,

and you're gonna see that I'm running

a tremendous number of containers right now.

One of the containers I am running

is the multi-client image.

So this is the container that is running

inside that pod right now.

Now, I'm gonna take that container ID.

Again, this is a container that's running inside that pod.

I'm gonna take the ID,

and I'll do a docker kill with that ID.

So this is going to completely kill

that running container inside of that pod.

So that's it.

It is done, it's gone.

Now, I'm gonna do a docker PS again.

If I scroll all the way back up,

you're gonna very quickly see that it looks like,

hey, wait a minute, it's still there.

There is Steven Grider multi-client.

That is the container running inside that pod.

But you'll notice that the thing

was only created two seconds ago.

If I also run the command "kubectl get pods,"

you'll notice that there is now one restart

reflected inside of here as well.

And so the thing that I wanted to demo to here very quickly

that's going to kinda play into the diagram

that we're gonna look at in just a second

is that it appears that if we manually delete,

or if one those containers inside that pod crashes,

it appears that it automatically

gets restarted for us for free.

So with that in mind, we're not gonna look

at a rather complex diagram.

We're gonna walk through the process of what happened

when we deployed or applied, I should say,

these two configuration files.

Now, just a quick disclaimer, this is going to be

a rather long video as we go through this discussion.

So I don't know, pause it at some point.

Take a break if you feel like it.

I don't know.

Okay, so this diagram, super boring, not enough detail.

I wanna look at this right here.

All right, so this is a little bit more realistic diagram.

We're gonna walk through this thing step by step

and see what happens when we deploy one of those files.

Now, on the right hand side,

you'll notice that I'm reflecting three separate nodes.

Remember, a node in the world of Kubernetes

is a computer or essentially a physical computer

or a virtual machine that is going to run

some number of objects that we create inside of our cluster.

You'll notice down here at the bottom center,

I have something labeled called the "master".

On the left hand side over here,

we've got a deployment file.

So the deployment file is essentially identical

to the two files that we just put together

that create a pod or a service.

And then finally, up here on the top left hand side,

I have the docker hub that lists a couple

of the different images that we have created

throughout this course.

So let's walk through a series of actions now.

We're gonna start off with our deployment file down here.

Now, we're going to imagine that rather than creating

a single instance of the multi-client image

or multi-client container

maybe we have a deployment file that instead

says that it wants to create four copies

using the multi-worker image.

So we're just gonna imagine, for the sake of discussion,

that we slightly change our deployment file.

We're then going to imagine that we take this file

and we feed it into the kubectl apply command.

Remember, that is the command that we use

to load up the two files

that we had previously put together,

the client pod and client node port.

So when we run this command right here,

the file is taken,

and it is passed off to something called the master.

Now, on the master,

there is a variety of different programs,

three or four in total that control

your entire Kubernetes cluster.

So there are four programs in total,

but you and I are just going to kind of imagine

for right now that there's just one program

called the Kube API Server.

Again, four programs.

We're just gonna kinda consolidate it all down to one.

The Kube API Server program is 100% responsible

for monitoring the current status of all the different node

inside of your cluster and making sure

that they are essentially doing the correct thing.

So we're going to imagine we take this deployment file

and pass it into the master.

Kube API Server is gonna see this new file,

it's gonna read the configuration file,

and it's going to interpret it in some fashion.

The master then has a little kind of notepad of sorts

that records all of its responsibilities

or essentially, all the things that you and I

have told it to do in the form of these deployment files.

So when we feed in the to deployment file,

Kube API server is gonna look at that file

and it's gonna see, oh, all right.

The developer wants us to run four copies of multi-worker.

And so Kube API Server,

it's going to update its little list

of responsibilities and say,

"Okay, I need to be running an image

called multi-dash worker.

I need to be running four copies of it.

And right now, I am running zero copies."

So Kube API Server is gonna update its little list

of responsibilities right here, and it's gonna say,

"Oh, boy, I need to be running four copies,

and I'm currently running zero."

Well, that's definitely bad news.

So what's gonna happen?

Kube API server is then going to reach out

to the three different nodes that are running,

and it's gonna say to each of these,

"Hey, people, look, we got a big problem.

We need to be running four copies of multi-worker,

but right now, we are running zero.

So you three nodes, I want you to start up

some number of copies of multi-worker."

It's gonna say to the first node down here,

"I want you to start up two copies of multi-worker.

I want you to start up one copy,

and I want you to start up one copy as well."

Now, inside of each of these nodes,

there is a copy of docker running.

That's right, a copy of docker running

inside of each of these virtual machines.

That is true of the virtual machine that is running

on your computer right now with Minikube as well.

So technically on your computer,

you now have two copies of docker running,

both the copy created by Docker for Windows

or Docker for Mac and the copy that is running inside

of that single node that has been created by Minikube.

Now, when the master down here says you need to create

two copies of multi-worker, if one copy, one copy,

the copy of docker inside of each of these different nodes

is going to spring to action.

It's going to reach out to docker hub,

and it's gonna find the multi-worker image right here.

It's going to copy or download that image

and store it on some local image cache

inside each of these nodes.

Now, the thing to remember here is that

each of these copies of docker are 100%

kind of autonomous, not connected.

So they're all gonna reach out and grab that.

Oops, not multi-client, but multi-worker image.

And so you can kind of imagine

that we get the multi-worker image inside

of each of these nodes like so.

Then each node is going to use that image

to create a new container out of it.

So we're gonna create one copy

of multi-worker as a container.

We're gonna make another copy down here.

And then we had said that this last node

would make two copies of the container like so.

Now remember, technically, each of these containers

are being ran inside of that pod interface

or that pod object thing.

But for the sake of this diagram,

we're just gonna say, "Yeah, the containers basically exist,

but they are inside of pods nonetheless."

So now that each of these have started up

a copy of multi-worker, the master is gonna reach

back out to each one of these nodes,

and it's gonna say, "Okay, need a status update.

How are things going?"

It's gonna look at the first node,

it's gonna see, okay, one copy.

It's gonna look at the second node, okay, one copy.

It's gonna look at the third node.

Great, two copies.

So in total, it now has four copies of multi-worker running.

And so it goes back to its little

list of responsibilities of sorts over here.

And it's gonna say, "I now have four copies

of multi-worker running."

And at that point, the master says,

"All right, that's pretty much it.

I'm gonna sit back and relax because I've done my job."

So that's pretty much the entire flow.

Now, the last thing I wanna mention

is what happened when I did that

Docker PS command over here?

And then you'll recall I killed that running container

with docker kill on this ID right here.

So what exactly happened there?

Well, just a second ago when I said,

"Oh yeah, the master sits back and relax."

Well, that's not what really happens.

You see, the master is always continuously

pulling each of these different nodes.

It's watching every single one.

And anytime that something happens

inside one of these nodes,

the master gets a little notification.

So we can kinda imagine that when I ran docker kill

and then I killed that container,

we can imagine that one of these containers

essentially fell away like so.

So the master then got a little notification that said,

"Hey, little issue here.

One of our containers just up and died.

Completely gone."

And so the master very temporarily updated

its little list of responsibilities to say,

"Okay, I need four copies of multi-worker,

but I now have three copies running."

Whoa, that's a big problem.

So the master then looks back out

at its collection of nodes and it says,

maybe it picks this node right here and says,

"Hey, you, you need to have an additional copy

of multi-worker running."

And so this node right here will use that image

that it's already downloaded,

and it'll create a new container out of it.

So multi-worker container, and then it reports back

to the master and says, "Hey, we're good.

I just started up another copy."

And so the master says, "Great, I now have four copies

running again, and I am plenty happy."

All right, so that is the full flow.

That is what is happening behind the scenes.

Now, some of the big takeaways that I want you

to understand here from all this stuff, first off,

when you and I loaded up that deployment file,

we did not pass that deployment file

directly off to one of these worker nodes.

Instead, the deployment file went to the master.

So the big lesson here is that you and I

as developers work with this master.

You and I do not work directly with the node over here.

In other words, you and I are never going to reach

into a node with some series of commands

and attempt to manually start up a container

inside of one them.

Instead, we're always going to use the kubectl

command line tool, which is going to send

all of our commands off to the master.

It's then up to the master and the different programs

that are running inside this thing to reach out

to some appropriate node

and create the appropriate container,

or essentially, update the state, or excuse me,

update the appropriate container,

update the appropriate node.

It's up to the master to reach out to some node

and tell it to do some amount of work to fulfill

the master's little list of responsibilities.

The next big thing to take notice of

is that the master is always watching

each of these different nodes.

Anytime that some container or some object

for that matter runs into some issue,

the master is going to automatically attempt

to recreate that object inside of some given node

until it's list of responsibilities

is 100% balanced out.

Now, that kind of idea of kind of saying,

"Hey, master, here's your list of responsibilities."

And then having the master constantly work to make sure

that that list of responsibilities is fulfilled

is one of the most important ideas

in everything around Kubernetes.

Yes, just as important as that other thing I said

was really important, the networking stuff.

Yeah, there's a lot of important stuff.

You get the idea.

But in reality, or all joking aside,

the idea of the master constantly working to fulfill

this list of responsibilities right here is very important.