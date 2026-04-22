-: In this section,

we're going to very quickly put the configuration together

for our multi worker deployment.

After that, we're going to make sure

that we take all of our different config files

that we've put together,

and throw them all back into Cube CTL,

through that Apply command,

and just get kind of a general idea

of where all of our work is at.

So, let's put the configuration together

for the multi worker pod.

As you might guess, this file is gonna look very similar

to the config that we put together

for the other two deployments.

So inside my K8's directory,

I'm going to make a new file

called WorkerDeployment.eml.

Then inside of here, we'll put together config very similar

to the other deployments.

We'll say API version is going to be apps view one.

The kind of object that we are creating is a deployment.

We'll give a metadata section

to this thing with a name of worker-deployment,

and then we'll provide a spec to this thing, as well.

So for replicas,

we're gonna start off with just as single replica,

or a single pod running the multi worker image.

Remember that if there's anything inside

of our application that really needs to scale out

and have multiple instances,

it probably is the multi worker

because that was the image or the container inside

of our application that was doing that fibonacci calculation

and that was the absolutely slowest part

of our entire application.

And so at one point in time, or at some point in time,

we might want to come back and put a little bit more work

into that thing to make sure that it is scaled properly.

But for right now, we're gonna start off with just

a single replica, so that we can eventually see that,

yeah, this thing needs to be scaled and we can figure

out how to do that at that point in time.

All right, so after that, we'll provide our selector.

We'll say that we want to attempt to match a label

on some other object inside of our cluster.

And for this one, we'll give a component of worker

because that's really its job.

It is a worker container inside of our application.

After that, we'll put together our template

for the pods that are created.

Remember that the template property goes

on the same indentation layer as selector.

We'll provide a metadata to the template,

where we'll provide the different labels

that need to be applied to every pod that gets created.

And of course, we're going to provide a label

that matches the selector

that we had specified just a second ago.

So under labels, I'll say component worker.

After that, I'll un-indent back out to the metadata section

or metadata kinda indentation layer, I suppose,

and will provide a spec that's going to configure exactly

what this pod is running.

So we're going to say

that we want to run a single container,

but remember it is plural containers, like so.

And then the single container that we want to run

is going to have a name of worker.

It's going to use the image of your Docker ID, multi worker,

and then just as before,

we are going to eventually need to stuff in

some environment variables into this thing

to tell it how to connect to Redis.

But for right now,

we're just going to kind of ignore that for a second.

We're going to first set up Redis

and make sure that's working correctly.

And then we're going to come back

to both our worker and express API deployments

and add in the environment variables at that point in time.

Now, the other thing I wanna mention here is,

if you look at this diagram,

remember there is nothing inside of our worker image

or the worker container that's running here

that needs to be accessible

from anything else inside of our cluster.

In other words, there's no other object whatsoever,

no other service, no other image, no container, no nothing,

that needs to directly connect to the multi worker

and try to get some information out of it.

The multi worker is going to connect

to something else inside of our application,

but nothing is going to make a unprompted request

into the multi worker.

And as such, the multi worker does not need

to have any port assigned to it,

and it does not need to have

any service assigned to it either.

We only make use of services

when we want to have requests going

into a set of pods or into a single pod for that matter.

And because that's not the case here, no need for a service,

no need to worry about any ports.

Okay.

So with that in mind,

that's pretty much it for this file.

No ports required.

So we're just going to finish it off like so.

And we do not need to create a associated service file.

So let's save this as is,

we'll take a quick pause right here,

and when we come back to the next section,

we're going to take the three config files

that we just put together in the last couple videos,

and throw them into Cube CTL through the Apply command.


```
apiVersion: apps/v1

kind: Deployment

metadata:

	name: worker-deployment

spec:

	replicas: 1

	selector:

		matchLabels:

			component: worker

template:

	metadata:

		labels:

			component: worker

	spec:

		containers:

			- name: worker

			  image: stephengrider/multi-worker
```
