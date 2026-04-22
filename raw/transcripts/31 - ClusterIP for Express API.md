
```server-cluster-ip-service.yaml
apiVersion: v1

kind: Service

metadata:

	name: server-cluster-ip-service

spec:

	type: ClusterIP

	selector:

		component: server

	ports:

		- port: 5000

		  targetPort: 5000
```

Instructor: In the last section,

we put together our deployment file

for the multi-server set of pods,

and we made sure that we specified that port 5,000

should be accessible on the image

that gets created inside of each of those pods.

So we're now gonna create a second configuration file

that's going to serve as what's going to create

and maintain the cluster IP service.

So I'm going to again, flip back over to my code editor

and inside of my K8s directory,

I'm going to put together a new file

that I will call server cluster IP service dot YAML.

All right, and I'm gonna, or actually I don't need to,

I was gonna say, I was gonna type out the name of the file

right here just so you can see the whole thing

but you can actually see the whole name

right here on the tab inside of my code editor,

just in case you want to reference the exact name

that I'm using.

All right, so inside of here,

we're going to put together another config file

for a service of type cluster IP.

This is again, going to be just about identical

to the previous cluster IP service we put together

but let's go through typing it all out again

just to kinda commit this stuff to memory.

It'll be pretty quick.

So I'm going to provide an API version of V1.

I'll provide a kind of service or our metadata.

We're going to specify the name

of this service or this object.

And I'm going to use a name

of server cluster IP service, like so.

And then we will provide a spec

that's going to customize exactly how this server behave

or (indistinct) how this service behaves.

So we're going to specify a type of service of cluster IP

and then we need to provide a selector

that's going to tell this service exactly what set of pods

it's supposed to provide access to.

And so of course the cluster IP

that we're putting together right now

is supposed to provide access to all the multi-server pods.

In just the last section, when we put together

the server deployment file, we had assigned

each of those pods a label of component server.

And so inside of our server cluster IP service,

we're going to want to specify a selector

of component server.

So back over here,

I'll provide a selector of component server.

And then finally, very similar to what we did last time,

we're going to make sure that we specify the ports

that this service is going to kind of mitigate control over

or managed control over.

So just as before,

we're going to just send everything through.

And as a reminder,

the multi-server image listens for traffic on port 5,000.

So we're going to provide our ports

with a port of 5,000 and a target port of 5,000 as well,

just as we did inside of our previous service.

Okay. So that's it.

That's all we have to do to put together

our cluster IP service that's going to govern

or manage access to our running express API.

So let's take a quick pause right here.

I want to address one little side topic

and then we'll continue with a couple more config files

or the rest of all the objects inside of our application.

So quick pause and I'll see you in just a minute.