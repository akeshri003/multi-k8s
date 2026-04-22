
client-pod.yaml
```
apiVersion: v1

kind: Pod

metadata:

name: client-pod

labels:

component: web

spec:

containers:

- name: client

image: stephengrider/multi-client

ports:

- containerPort: 3000
```
client-node-port.yaml
```yaml
apiVersion: v1

kind: Service

metadata:

name: client-node-port

spec:

type: NodePort

ports:

- ports: 3050

targetPort: 3000

nodePort: 31515

selector:

component: web

```

We're gonna first focus on the first two lines in each file.

So specifically API version V1, and kind pod.
### kind


You'll notice that the other file has the very similar

same two lines at the top,

but in the second file, we have kind of service.

So again, we're gonna focus on these two lines

to get started.

And just so you know, we're going to first,

between the two of them,

we're gonna first focus on exactly

what kind is referring to.

So just as we discussed a couple moments ago,

we had said that with Docker compose,

we had created a configuration file

to describe the different containers that we want to create.

And we had done something very similarly over

with elastic bean stock as well.

Now with Kubernetes,

we do make these config files to make containers,

but we had kinda honed in

on that definition with this diagram over here

and said that when we make a config file with Kubernetes,

we're not quite making a container,

we're kind of making something else slightly different.

We're making something called an object.

And so I first wanna focus on exactly what an object is

and help you get an understanding of what an object is.

So back over here.

So we've written two configuration files.

We're going to eventually take these

two configuration files we put together

and feed them into the kubectl command line tool

that we installed a couple videos ago.

Remember that is the kubectl command

that we had ran at our terminal just a moment ago.

Just a second ago, we ran cluster info like so.

So eventually we're going to take

these two configuration files and pass them

into this command line tool.

When we pass them in, kubectl is going to interpret

both those files and create two objects out of each file.

So the config files that we write

are going to use to create objects in general.

The term object is a reference

to a thing that exists inside of our Kubernetes cluster.

And so we don't specifically say

that we are making objects quite so much.

In reality, we are making specific types of objects.

And in this diagram right here,

I'm showing you a couple of sample object types.

So an example of an object would be a state full set.

Another example of an object would be a replica controller.

These are all things that we can create

inside of our Kubernetes cluster

that have very specific purposes

to make our application work the way we expect.

Two other examples of object types

that we're going to very frequently be using

is a pod and a service.

And you might have noticed

that inside of our two configuration files,

we have the word pod next to kind

and we have the word service next to kind as well.

So the first thing to understand here

is that the kind entry inside of all the configuration files

that you and I are going to write is meant to represent

or indicate the type of object that we want to make.

So in our clientpod.yaml file, we're making an object

of type pod because that's what we entered in

for the kind property.

In the client node port file, we are making an object

of type service because that's what we represented

for the kind property over here as well.

Now you might be curious, what exactly is an object?

What are they used for?

Well, like I just said,

they're essentially things that are going to be created

inside of our Kubernetes cluster

to get our application to work the way we might expect.

And so for example, every object that we're going to create

or every type of object has a slightly different purpose.

Some objects are used to run a container

as is the case with a pod.

A pod is used to run a container.

Other types of objects might monitor a container.

Other types of objects such as a service

are going to set up some kind of networking.

And so we made a object of type service

to set up some networking inside of our Kubernetes cluster.

![[Screenshot 2026-04-17 at 7.31.05 PM.png]]
![[Screenshot 2026-04-17 at 7.32.11 PM.png]]

**What an object is? What do you mean by making config files for object?**
We feed the config files we made to kubectl in the cli 

After we pass those config files, kubectl will make 2 objects out of those config files.
The term object is reference to a thing inside our k8 cluster. 
so we are not making specific object. we are making diff types of objects.
And we specified the the "type" of our object in our yaml files, using `kind` key, which gave an idea of about what kind of object this yaml file is about.

### API Version

Now let's talk about the apiVersion key in our config yaml files - 
![[Screenshot 2026-04-17 at 7.37.29 PM.png]]

Now I wanna tell you a little bit more

about API version V1 on both these files.

Let's see over here, here we go.

So when we specify the API version

at the very top of the file, that essentially scopes

or limits the types of objects that we can specify

that we want to create within any given configuration file.

So inside of both of our configuration files,

we specified an API version of V1.

That essentially opens up access

to us to a predefined set of different object types.

And so we have access or our configuration file.

In both cases, because we are designating API version V1,

we can create an object of type component status

or config map or endpoints or event or namespace

or pod or any of another of other object types

that I'm not reflecting inside this diagram.

If we had used a different API version

such as API version colon app slash V1,

we get access to a different set of object types.

And so if we had put in app slash V1,

we could have created an object of type controller revision

or an object of type stateful set.

Now in practice, the API version flag

is just a little bit annoying,

I'll be honest with you.

What you're usually going to do with any configuration file

you put together is you're first gonna decide

on what kind of object

or what type of object you want to create.

You'll then figure out what API group

that object type belongs to,

and then you're just gonna go look up

that API version and stick it into your configuration file.

So in other words, I knew that we had wanted to create

something to create a container

inside of our Kubernetes application.

And so based on my knowledge,

I knew that we needed to create a pod.

So what I did was I looked up a document that said, hey,

if you wanna make a pod, you have to be using the pool

of object types designated in API version V1.

And so that's why I put down

API version V1 inside my config file.

It's because ahead of time,

I knew that I had wanted to make a pod.

So the reason that I say

that this API version is a little bit annoying

is that it's mostly a reactionary feature

and you just gotta go do a little bit of reference

to figure out what you should be specifying.

Now throughout this course,

I'm gonna be telling you in great detail

about exactly what API version we're going to be specifying

for every config file we put together.

So you don't really need to worry about memorizing

these things that much throughout this course.

So at this point we've discussed what API version means.

We've also discussed what kind means

although we haven't really discussed what a pod is,

or really discussed what a service is.

So let's take a quick pause right here.

We're going to come back to the next section

and expand on exactly what a pod is.

So I'll see you in just a minute.