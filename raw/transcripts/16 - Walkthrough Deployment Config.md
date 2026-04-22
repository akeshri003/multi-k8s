![[Screenshot 2026-04-19 at 3.38.36 PM.png]]
![[Screenshot 2026-04-19 at 3.41.19 PM.png]]
![[Screenshot 2026-04-19 at 3.41.34 PM.png]]

-: In the last section,

we put together our first configuration file

to create a deployment.

In this section, we're going to

walk through the configuration file

and talk about a couple of interesting things

that's inside of here.

Now again, the first two lines that you see

specify the API version

and the object type that we want to create.

So in this case, we're making an object of type deployment.

After that, we see a very familiar metadata section

that names the object that's going to be created

by this configuration file.

So in our our deployment deployment will have a name

of client dash deployment.

Now down in the spec section,

I wanna skip on down to the template property right here.

So inside the template,

we're listing out the configuration

that is going to be used for every single pod

that is created by this deployment.

The entire template section right here

probably looks a little bit familiar

to some of the configuration that we put together

inside of our client pod dot yaml file.

So back inside of client pod, we had a label.

We had a list of containers assigned as well.

And so you can essentially imagine

that this template section right here

is defining the exact configuration that should be used

for every pod that is created

and maintained by this deployment.

Every pod that gets created by this deployment

will have a label of component web

and it will have a container with a name of client

and using the image multi client with a port 3000

mapped up to that container.

Now, besides the template right here,

the other properties that are kind of interesting

is replicas and selector.

So first off, replicas.

As you might guess, this is specifying

the number of different pods

that this deployment is supposed to make.

And remember, every one of those pods that it creates

are going to be absolutely identical in nature.

So at this point, we are saying,

"Hey, deployment, create exactly one pod

using this template down here."

If we wanted to, we could very easily change this up

to something like five.

That means that we would want to create five separate pods

each of which run this exactly identical template.

We're going to leave replicas as one for right now, however.

Now the other thing that's kind of interesting

inside of here is the selector.

The selector looks very similar to a selector property

that we previously put into our node port file.

So back over here we had a selector property as well.

Let's talk about what the selector is.

Essentially, it's doing something very similar

to what the selector

inside of the service configuration file was doing.

You see, when a deployment creates a pod,

it doesn't directly create the pod itself and maintain it.

Instead, the deployment reaches out

to the Kubernetes API on the master and it says,

"Hey master, I need you to create a pod for me

and here's the configuration for it."

The master then goes ahead and creates the pod.

Now it's the master or some program inside the master

that is creating the pod for us.

And so the deployment after the pod is created

needs to somehow get a handle on the pod that is created

and that's the purpose of that selector field.

The deployment says, "Okay, I'm gonna ask master

or some program in master to create this pod for me."

After it gets created, I need to somehow get a handle on it.

And so to get a handle on that newly created pod

I'm gonna look for objects with a label of component web.

And so of course

every pod that we are creating using this deployment

is going to have a label of component web

because that's what we specified inside of our template.

And so matched labels and selector right here.

This section right here is essentially giving us a handle on

this label down here.

I know it seems a little bit repetitive

to have to kind of like designate this stuff twice

inside the same file, but it's entirely possible

that we might have multiple labels assigned

to our deployment

or multiple labels assigned to the pod that is created

by our deployment.

And maybe we only want our deployment to look for

one or two very specific labels out of those.

So that's why you're seeing it

kind of replicated here twice.

Okay, so that's pretty much it

for our deployment configuration file.

So let's take a quick pause right here.

We'll come back to the next section

and we're going to apply this deployment file

to our Kubernetes cluster by using Kubectl.

And then we'll very quickly be able to

look at a couple of interesting things

around the deployment and the pod that is greeted with it.

So quick break and I'll see you in just a minute.