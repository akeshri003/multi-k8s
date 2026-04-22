```redis-deployment.yaml
apiVersion: apps/v1

kind: Service

metadata:

name: redis-deployment

spec:

replicas: 1

selector:

matchLabels:

component: redis

template:

metadata:

labels:

component: redis

spec:

containers:

- name: redis

image: redis

ports:

- containerPort: 6379
```

```redis-cluster-ip-service.yaml
apiVersion: v1

kind: Service

metadata:

name: redis-cluster-ip-service

spec:

type: ClusterIP

selector:

component: redis

ports:

- port: 6379

targetPort: 6379
```

  
![Lecture thumbnail](https://mp4-cdn77.udemycdn.com/2018-08-24_15-24-10-8998343bae1cc123cdca1a4b472d8399/2/thumb-sprites.jpg?secure=r1qHV_8yCLh_DgGp-a0HmQ%3D%3D%2C1776629887)

7:23 / 7:26

-: In this section

I've got some really good news and some bad news.

So the good news is that we get to put together

our configuration for the Redis deployment

that's going to manage a single Redis pod.

The bad news is that that means

we have to write out two more configuration files

very similar to the ones we've already put together.

So I know writing out all these configuration files

is getting very tedious,

but we're almost at the end here.

We just have to do the Redis deployment,

its cluster IP service,

and then the same thing for Postgres

and its service as well.

And then after that,

we're done with these very repetitive config files,

and we get to look at some more interesting config files

that are going to do some more interesting things

inside of our cluster.

So let's get to it.

We're gonna create our deployment config file for Redis

and its associated cluster IP service.

All right, inside of my code editor

I'll find my K8s directory.

And inside there, I'll make a new file

called redisdeployment.yaml.

So we're gonna start off first with the deployment.

Now the deployment that we're going to put together

is going to look essentially identical

to the other ones that we've already written so far.

So let's get through it as quickly as we can,

but do make sure that you type out everything correctly

because any little typo

is going to eventually result in some error message

down the line.

All right, so I'll do an API version of apps V1.

I'll give this object a type of deployment.

I'll set up a metadata.

That's going to provide a name of Redis deployment.

And then we'll start putting together some configuration

that's going to exactly configure

how the deployment is created and how it behaves.

So for my spec, I'm going to give a replicas of one.

We only want to have one single copy of Redis

at any given time.

Now Redis is very interesting,

in that we can set it up in a sort of cluster mode

where there will be multiple copies of Redis

that kind of communicate with each other

and enhance the overall stability

and throughput of our application,

but this is a course much more about Kubernetes

than it is about Redis.

So we're not gonna worry about setting up Redis

in any sort of like cluster mode or anything like that.

So we're just gonna have one standalone copy of Redis.

All right, We'll also give it a selector.

It's going to look for any set of pods out there

that has a label of component Redis.

I'll then unindent back out

to be on the same indentation level as selector,

and we'll provide our pod template.

So for the pod template,

we wanna give it some metadata.

Notably, we want it to have a set of labels

of component Redis like so.

I'm then going to unindent again

so that I'm on the same indentation level as metadata.

And we'll provide the pod a spec.

So it's gonna have a single container,

but remember the keyword right here is containers, plural,

even though we only have one.

So we'll give this thing a name of Redis.

It's going to use the image Redis

with no Docker ID required this time,

like your DockerID/

because we don't have our own custom version of Redis,

we are using the copy that is included

in the public repository over on Docker Hub.

We'll then set up the different ports

that need to be mapped to the container.

In the case of Redis,

the default port that it uses is 6379.

And there's really no good reason

for us to try to change that.

Totally fine to use the default port.

So I'll set up a container port of 6379 like so.

All right, again, I gotta ask you,

please double check all the indentation

and all the spelling inside this file,

because if anything inside of here is wrong,

like any little typo whatsoever

is going to eventually result in an error message.

Like literally, if I change anything inside of here,

like if I delete a P in component or something like that,

something down the line is not going to work as expected.

So please double check your spelling inside this file.

All right, so that's the deployment.

So we're now going to create a cluster IP

so that our server pods and the multi worker pod

can eventually connect to this Redis instance

that's running inside of a container inside of the pod.

So back inside of my code editor

I'll find my K8s directory.

I'm gonna make a new file called

redisclusteripservice.yaml.

And then inside of here,

we'll put together the config

to make a new cluster IP service.

And again, it's gonna be identical

to the other ones we've already put together.

So let's get through this quickly as well.

We'll do our API version of V1.

We are making a service.

It's gonna have metadata with a name of Redis service.

Actually I think that,

what was the terminology we've been using?

Cluster IP service.

So let's make sure that we stay very consistent here.

So I'll do Redis cluster IP service like so.

I'll then give it a spec with a type of cluster IP.

We'll make sure that we provide the selector

so that this service knows what set of pods

it is managing access to.

So it's going to be component Redis.

And then finally, we need to designate what set of ports

this service is going to manage.

So I'll say ports.

Any outside object that is trying to get at our Redis pod

is going to access this thing on port 6379.

And then after it goes through the service,

we're just going to let it stick with 6379

and have that port be what it connects to

inside of our container.

So I'll do a target port of 6379 as well like so.

Again, no good reason to try to redirect the ports here.

There are situations where you would want to do that.

For example, if we have an Nginx server

that is supposed to be serving up web traffic

on like port 80 or something like that,

but for whatever reason,

we have configured Nginx to listen on port 3000,

which is actually something we kind of did

on the React application,

but the React application is not directly receiving traffic.

It's kind of backed behind that Ingress thing

that we're going to eventually set up as well.

But if that were the case,

then we could do that little bit of port redirection

and not have to reconfigure our image or anything like that.

Okay, so this looks pretty good.

Now the last thing I wanna do

is load this up into kubectl as well with the apply command.

So I'll flip back over to my terminal.

There's the K8s directory.

I'll do a kubectl apply-FK8s,

where it's gonna throw everything inside there.

We should probably see a bunch of different objects

that are going to end up unchanged like so.

But then the two new config files that we just made

will be reflected as objects created inside this log.

And so just as before,

we can do a quick get pods

and verify that yep,

we've got a single copy of Redis up and running.

And we can also do a get services

and verify that our Redis cluster IP service

is up and running as well on port 6379.

Okay, so this looks good.

Now we've only got one last set of config files

for Postgres along with this associated cluster IP.

So let's get through that in last section.

I know this stuff is so tedious

with all this writing of identical files,

but we're almost done with that.

So let's finish up these last two config files

in the next section,

and then we get to get back to some more interesting topics

in the world of Kubernetes.

So quick pause and I'll see you in just a minute.