```client-cluster-ip-service.yaml
apiVersion: v1

kind: Service

metadata:

	name: client-cluster-ip-service

spec:

	type: ClusterIP

	selector:

		component: web

	ports:

		- port: 3000

		  targetPort: 3000
```

Instructor: In the last section, we spoke about

the purpose of a cluster IP service.

So we're now going to flip over to our code editor

and create a new config file that's going to expose access

to our set of multi client pods

to every other object inside of our cluster.

So, over inside of my code editor,

I'm gonna find my k8s directory

and I'm going to make a new file called

client cluster IP service.emo.

Then inside of here,

we'll put together a little bit of configuration

that's gonna look very similar to the config file

for the previous service we had created.

One was the node port,

this one is a cluster IP.

At the end of the day,

their config files look awfully similar.

All right. So inside of here,

we'll first start off with an API version of v1.

We're going to create an object

with a type of service for our metadata.

We're going to provide a name of client cluster IP service.

And then for our spec,

that's going to configure the way that this service behaves,

we're going to specify a type of service of cluster IP

and take note of the capitalization here,

we got capital C and capital IP on the end.

Next, we need to provide a selector

so that this service that we are creating

knows what set of pods

it is supposed to be providing access to.

So in our case,

the cluster IP that we are working on right now

is going to provide access to our multi-client set of pods.

So I need to make sure that we provide a selector,

so a selector that's going to properly select

our client deployment.

If you look inside of our client deployment,

you will recall that all of those pods

have a label applied to them of component web.

So the selector inside of our service will be component web.

Finally, we're going to configure the different ports

that the cluster IP service are going to expose

and be available on.

Now, the ports ruled down here

is going to follow the exact same nomenclature

as the node port,

stuff that we had worked on previously.

So you remember looking at this diagram?

This was the nomenclature that we had

for the node port service.

So at the node port, we had to provide a port,

a target port, and a node port,

and each of those different properties

had a slightly different purpose.

Now in the case of a cluster IP,

which is what we're making here,

there is no node port property.

Because this cluster IP is not addressable or accessible

from the outside world.

We are only going to have a port and a target port.

The port property is going to be how other pods

or other objects inside of our cluster

are going to access the pod

that we are kind of governing access to.

And the target port

is going to be the port on the target pod

that we are providing access to.

So, for us, we're going to provide a port of 3000

and a target port of 3000.

Now you'll notice that

I just left these things here the same.

If we wanted to, we could say that

you can get access to port 3000 inside that container

by trying to access something like,

I dunno, 30 50 if we wanted to.

So now, if any outside object wanted to access

the multi client pods,

they would have to try to go through port 30 50.

But honestly, I can't think of any good reason

in this particular case

that I would want to like do some mismatching of ports here.

So I'm just gonna leave them as the same

and say that to get at port 3000 inside the container,

you are going to get at port 3000 on the service.

So if you want to, you can kind of

do some redirecting of ports here,

but again, I don't really see a good reason to

in this particular case.

All right. So that's it for our cluster IP service.

So let's take a quick pause right here

and we'll continue with our next deployment

in the next section.