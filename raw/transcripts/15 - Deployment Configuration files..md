
-: In the last section,

we started talking about deployment objects.

Remember, a deployment is going to run and manage

a set of identical pods.

From this point forward throughout this course,

we are no longer going to create pods ourselves manually.

Instead, we're going to create deployments

that will create pods for us.

So deployments are appropriate for use

in both a development and production environment.

We're going to make a deployment that has a pod template

that's going to try to create a container

using the image multi-client,

and we're going to make sure that port 3000 is exposed.

Now the reason we're going to make a deployment

with multi-client and port 3000 open,

is so that we can test this out in the browser

once the deployment creates our actual pod.

So, to make a deployment,

we're going to use the same process that we use

for every other object

that we're ever going to create with Kubernetes.

We're going to make a configuration file,

and then feed that configuration file into kubectl.

So inside of my code editor,

I'm gonna create a new configuration file

inside of my root project directory,

and I'll give this file a name

of client-deployment.emo.

And then inside this file,

we're gonna add all of our configuration

to describe this new deployment that we're making.

We're going to write out

all the configuration together right now,

and then we'll talk about the purpose

of every line immediately after that.

So I'll get started by writing out API version,

and I'll say apps/V1.

I'll specify kind of deployment.

So these two lines right here.

I bet you can guess what's going on.

We are saying that we want to use a object

that is defined inside the API version of apps/V1,

and the specific type of object

that we want to create is a deployment.

Next up, we'll put down our metadata section,

and we'll give the thing a name of client-deployment.

Then we'll do our spec after that.

For the spec,

we'll say replicas is one.

We'll add on a selector with match labels,

and a component web property lexo.

I'll then un-indent

so I now only have one indent right here.

I'll say template.

I'll put in metadata,

labels,

component web.

I'm going to un-indent again

so that I'm on the same indentation layer as metadata.

I'll say spec,

containers,

and we'll define the list of containers

that we want created with every pod

that is controlled by this deployment.

So this is a array entry.

So I'll put in my little dash like so.

And I'll say that I want a pod that has a name,

or so be a container in this pod with a name of client.

I want it to run the image of my docker ID/multi client.

And then finally we'll set up that port mapping.

So I'll say ports

with a container port of 3000 like so.

Okay. So, something's inside this file

might look a little bit familiar.

Let's take a quick pause.

We're gonna come back the next section

and start breaking this thing down line by line.

So I'll see you in just a minute.

```
apiVersion: apps/v1

kind: Deployment

metadata:

name: client-deployment

spec:

replicas: 1

selector:

matchLabels:

component: web

template:

metadata:

labels:

component: web

spec:

containers:

- name: client

image: stephengrider/multi-client

ports:

- containerPort: 3000
```

![[Screenshot 2026-04-19 at 3.35.07 PM.png]]