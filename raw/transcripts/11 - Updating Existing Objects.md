
![[Screenshot 2026-04-19 at 2.46.24 PM.png]]
![[Screenshot 2026-04-19 at 2.46.52 PM.png]]

![[Screenshot 2026-04-19 at 2.49.24 PM.png]]

-: In the last section,

we had a very long discussion about the differences

between an imperative and a declarative deployment.

In this section, we're going to put a little bit

of practice into this and get a very practical example

of how we might update an existing object

using a declarative approach.

So what are we gonna update?

Well, we're gonna say that our new goal,

the old goal was to just create a container

inside of our Kubernetes cluster.

We're gonna say that our new goal is to update

that existing pod that we created

to use the multi-worker image,

as opposed to the current multi-client image

that it is currently using.

So essentially, what we want to do is find

that existing pod and update the image that it uses.

Now let's take a quick thought on how we would do this

with a imperative approach and a declarative approach.

So if we took an imperative approach to this,

we might do something like the series

of steps you see on the left hand side.

We might run a command to list out all the different pods

that we have inside of our cluster right now.

And then maybe we would issue a follow

up command to like take an idea or something like

that out of the current running pod

and tell that thing very explicitly

that it should now be running a new image.

So in this imperative approach over here,

we would have to essentially get the current state

of our application by running a command to list

out the current pods and find some idea

of the current pod that is running the multi-client image.

We then have to figure

out a migration strategy on our own to update

from the current state to our desired state.

Now in this case, figuring out that migration strategy

is pretty straightforward.

We could probably look up some command API reference

and figure out how we could tell a pod

to update the image it uses.

But again, that would take a little bit of work.

So I'm gonna suggest

that we instead use a declarative approach.

With a declarative approach,

updating and existing object inside

of our Kubernetes cluster is always going

to be the exact same process every single time.

So we're going to go and find the config file

that originally created that pod,

and we'll then make an update to it.

The update is going to simply say,

hey, I want to use a new image.

So inside the config file that we used to create the pod,

we're going to no longer say,

I want to use an image of multi-client.

Instead, I now want to use a new image of multi-worker.

Once we make that change to the config file,

we're going to just throw it into kubeCTL

at the command line and Kubernetes

is going to work to update

that existing pod to use the new image.

Now, one thing that's kind of interesting about the process

of the declarative approach over here

is that we're essentially saying

that we're going to take this config file,

throw it into kubeCTL, which is of course

gonna forward it onto Master, and somehow magically

the Master is going to realize that it needs to

update this existing pod as opposed to creating a new one.

That's actually something that's kind of interesting.

We're just gonna update this config file.

And you might notice that inside this config file,

there's really no unique ID or anything like that.

There's nothing that says,

hey, go and find this existing pod,

like a pod we already created and update it.

And so it might be a little bit confusing to figure

out when making a change to a configuration file

will update an existing object,

as opposed to just deciding to create a completely new one.

So behind the scenes, here's what Kubernetes does

anytime that you toss in a configuration

file into kubeCTL and Master.

So inside of our config file,

we're always gonna have an object, or we're always

going to be creating an object that has a name.

So every single last object that you

and I ever create with a config file is always

going to have a name assigned to it.

And in our case, we assigned a name of client dash pod.

In addition, every single config file we ever

put together is going to have an object

type assigned to it as well.

And technically it's not like labeled in there as the type,

it's really labeled as the kind recall.

Remember, like right here on line two we say,

kind pod, not object type, but you get the idea.

Every config file is going to list

out the type of object that we want to create.

So every single time that we take a configuration file

and pass it in the kubeCTL,

the Master is going to look at that configuration file,

and inspect its name and kind properties.

It's then going to look at all the different

running services inside the cluster and it's gonna say,

okay, if I have any other object inside

of here with a identical name, and in this case we do,

and an identical type, then that must mean

that rather than trying to create a new object,

I'm going to try to take the updated configuration file

and apply all these changes to the object

that already exists inside the cluster.

So in other words, the name

and the kind are our unique identifying tokens

for any object that we create.

If you ever want to update an existing object,

you're going to take your existing configuration file.

You're going to always leave the name the same,

and the kind the same, and you can change

the rest of the configuration file

and then feed it all into kubeCTL,

and Master is going to automatically

find the existing object and make updates to it.

Now, if we made a change to the name,

so if we said client pod, I don't know,

new or something like that, well it now has a new name.

So Master's gonna say,

okay, do I have an existing object that is a pod

with the name of client pod new?

And in this case, we do not.

There's no existing object inside of our cluster

with that same name and same object type.

And so Master would say, okay, well this is a new name.

I guess I need to create a brand new object

with that name and that type.

So in short, anytime that we want to make

an update to an existing object,

we just go and find the original configuration file.

We'll leave the name and the kind the same,

and then we can make any other change

we want to inside that file.

So with that in mind, let's take a quick pause right here.

We're gonna come back the next section.

We're going to make an update to our configuration file

and then feed it into kubeCTL.

And we're going to expect

that it's going to update an existing pod,

as opposed to attempting to create a brand new one.

So quick break and I'll see you in just a minute.