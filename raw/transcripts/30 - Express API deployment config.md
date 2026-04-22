![[Screenshot 2026-04-19 at 8.45.18 PM.png]]
![[Screenshot 2026-04-19 at 8.46.02 PM.png]]

```server-deployment.yaml
apiVersion: apps/v1

kind: Deployment

metadata:

	name: server-deployment

spec:

	replicas: 3

	selector:

	matchLabels:

		component: server

	template:

		metadata:

			labels:

				component: server

		spec:

			containers:

				- name: server

				  image: stephengrider/multi-server

				  ports:

					- containerPort: 5000
```


-: In the last couple of sections

we finished up our deployment configuration file

and the new cluster IP service to go along with it.

We're now going to continue by putting together

two more almost identical configuration files.

This time all centered

around our express API or the multi-server image.

Now we're going to make sure that we do a very

similar cluster IP service this time around

because we do not want the express API to

be directly accessible from the outside world

as would normally be the case with a node port service.

Remember, we use the cluster IP anytime

that we want to make a set

of pods only accessible to other things inside of our node.

The ingress service that we're going to eventually

put together qualifies as another thing inside of our node.

All right, so one quick thing I want to remind you

about here is

that our multi-server image is always going to be listening

for incoming requests on Port 5,000.

And that was something that we had really hard coded

into the image itself

and the actual server that's running inside there.

So when we create our cluster IP service

we're going to make sure that it's going to forward traffic

onto port 5,000 on each of those different pods.

And again, just to keep each port the same on both side

we're going to say that you can access the cluster IP

by accessing 5,000 as well.

Just as I said previously, there's really not a great reason

at first glance to like for some reason change

the port that the cluster IP accepts traffic on.

So we're going to make sure that it's 5,000 in both cases.

All right, so let's get to it.

I'm gonna flip back over

to my code editor inside of my K eight's directory.

I'm going to make a new file

and I'm going to call this server dash deployment dot yamo.

And then inside of here, we're going to put together

some configuration that's going to look very, very similar

to what we did in our client deployment just a moment ago.

So we're again going to say API version apps v1.

We're going to have a type of deployment for our metadata.

We will provide a name of server deployment

and then we'll start to add on the spec That's going to

customize exactly how this deployment behaves.

So for my replicas, replicas, let's make sure we

got the spelling correct.

Again, this is the number of different pods that

we're going to set up.

We're just going to arbitrarily say that we want three

pods running the multi-server image.

We don't really know a whole lot about how to most

appropriately scale these sets of pods yet.

So maybe three is a good place to start.

Maybe it's not, I don't know.

We'll find it out when we actually deploy this thing

and start to test our cluster with a little bit of traffic.

Next up, we're going to specify the selector

that the deployment is going to use to find the set of pods

that it's supposed to control.

So we're going to say that we want to match a set of labels

and then again, we'll do a component, but this time, rather

than doing web as the value for the component key, I'll

Do server like so.

And so, this is very clearly indicating

that the component inside of our application

that we are working on right here is the server.

I want to remind you that the labels we put together

can be anything you can possibly imagine.

So we could just as easily do something

like part of app API or something like that.

But I personally enjoy this kind of component.

And then piece of the application terminology, I think that

it's rather clear what this key value pair is indicating.

Okay, so now that we got that put together

we're gonna go back out in indentation to be

on the same indentation layer as the selector and replicas

and then we will put together our pod template.

So we'll first begin with our metadata, excuse me, metadata.

That's going to specify our set of labels.

And again, the labels that we apply to the thing must

at least match up with whatever we put inside

of the spec for the deployment itself.

So for labels, I'll say component server like so.

All right, I'm going to indent back out a little bit more.

So now I am on the same indentation level as metadata

and we're going to provide the spec that's going to

customize the exact behavior of each

of the pods that gets created.

Now we're going to again, provide a list

of containers that this pod is supposed to run.

Just as before

we are only going to have one single container

inside of this pod.

We don't have any other containers that need to run

along with the multi-server image.

So I'll give a name of server.

The image is going to be my docker ID slash multi-server.

And then the last thing that we're going to do

for right now is to add

on the list of ports that we want to make available.

So for ports, I'm gonna say that we want to

have a container port accessible of 5,000 like so.

All right.

Now I wanna give you a quick reminder.

The multi-server image that we put together

does expect to get a handful of key value pairs along

with it or environment variables that tells the multi-server

or the express API exactly how to connect

to our Postgres instance and the Redis instance as well.

So everything that we've put in together

inside this file at this point

in time is definitely stuff we have to add in here.

But we are going to eventually come back

and add on a series of environment variables

that we want to make sure get injected

into the container that is created with this image.

And remember the entire goal that is that we're

gonna tell the express API how to connect

to Postgres and Redis.

All right, so we are going to come back to this file

but for right now I want to just focus

on finishing up this very specific piece

of our application set up for right now.

And I don't wanna think just yet

about how we're going to involve Redis

in Postgres in here just yet.

Okay, so let's take a quick pause right here.

When we come back to the next section

we're going to put together our configuration file

for the cluster IP service.

So quick pause and I'll see you in just a minute.