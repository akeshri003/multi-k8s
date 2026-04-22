![[Screenshot 2026-04-19 at 5.23.36 PM.png]]

Instructor: In the last couple of sections,

we've looked at a couple of different ways

of updating a deployment.

So we understand that we can change the number of replicas

and we can change, say the container ports

or even the image that is used

by this deployment whenever it is creating a pod.

But one thing that we haven't quite looked at

is how we can update a deployment

when a new version of an image becomes available.

So I want you to imagine for a second

that maybe you and I

go off to our multi worker project

and maybe we make an update to that image

and then we push the image up to Docker hub.

How would we somehow

get our deployment to pull down that latest version

and recreate all of our pods

using that latest version?

I wanna look into that a little bit inside this section.

Now let's take a look at a diagram

that's going to explain

the series of changes we're going to make to our project

to kind of simulate updating an image

and kind of simulate how we would update a deployment

to accommodate for that updated image.

Okay, so first off, we're going to update our deployment

and we're gonna change it to use the

multi-client image again.

The only reason that we're going to change it to use

multi client

is that it gives us something to look at inside the browser.

And so it makes it really easy to verify that

our pods are running the correct version of an image.

After that, we're then going to go

back over to the source code for our

multi client project.

We're gonna make a change to that project.

We're gonna rebuild the image

and then push the image up to Docker hub.

So that's gonna simulate

a very real deployment

or a very real change to an application.

All the time when you're using Kubernetes

and all this container stuff

you're going to want to change an image

and you're going to want to make sure that the

latest version of that image

eventually gets deployed onto your Kubernetes cluster.

After we push that changed image up to Docker hub

we're then going to somehow get our deployment

to recreate all of its pods

with that latest version of

multi client that you and I create.

So let's get to it.

Step number one is going to be to

change our deployment to use

multi client again.

So inside of my code editor

I'm gonna find the client_deployment.emo file.

We're gonna make a couple changes in here.

The first thing I'm going to do

is reduce the number of replicas

down to one.

So we're just gonna run

one replica all by itself.

That's just gonna make it a little bit easier

for testing purposes, so that we can

get a better idea of when that

single pod is being updated or recreated

or whatever it might be.

Next up, we're going to change our image right here.

So instead of multi worker,

we're going to get multi client.

And then one other thing that I want to do

on the image right here,

besides putting on multi client

is I wanna make sure that we also update the port

that is exposed or forwarded on that image.

So a container port rather than nine nine nine,

we're going to change that back to

3000 like so, because you remember

that is the port, that is exposed or

that engine X inside the multi-client image

is going to listen on.

Okay, so that looks good.

Let's now

take these changes.

I'm gonna save this file

and then I'm going to apply

this new configuration

to our Kubernetes cluster.

So back at my terminal, I'll do a cube CTL apply

client_deployment.emo.

So I'm gonna make that change.

And then we will want to

very quickly test this out inside the browser

and make sure that we can access

that newly created container

that's inside of that new pod.

So remember, anytime they want to

access our app inside the browser,

we need to get our mini cube IP

with the mini cube IP command.

I'll copy the IP address

then we'll go back over to our browser.

I'll paste in that IP and remember

we set up that service,

to listen on port 3 1 5 1 5.

So I'll do colon

3 1 5 1 5

And then I see our application appear.

Okay, so that's easy enough.

Let's take a pause right now.

When we come back to the next section

we're gonna make an update

to the multi client image

and then we're going to push it up to Docker hub.

So quick pause and I'll see you in just a minute.