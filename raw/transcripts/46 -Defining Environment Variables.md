![[Screenshot 2026-04-20 at 10.55.23 AM.png]]![[Screenshot 2026-04-20 at 10.55.58 AM.png]]

YELLOW - consistent constant values
RED - also gonna be constant values, but they are basically url of the sorts used for establishing connections with them. We need to tell server pods, how to connect to pg-deployment and redis-deployment using CLusterIP. 
![[Screenshot 2026-04-20 at 10.57.50 AM.png]]

so to establish the connection between the worker and the redis-deployment, we need to provide the url or connection ip of the clusterIP service of the redis deployment. 

  
![Lecture thumbnail](https://mp4-cdn77.udemycdn.com/2018-08-24_22-45-47-0a3793007acdb2a2aed1ffdaa1d56127/2/thumb-sprites.jpg?secure=vUS6YzQrFHGbgLBOBlOQtg%3D%3D%2C1776678881)

4:05 / 4:09

-: One of the last things we have to do

to set up our application in Kubernetes

is set up some different environment variables.

So as a quick reminder,

we've got a handful of environment variables

that need to be assigned to both the multi-server

and the multi-worker.

The multi-worker needs to know how to connect to Redis

through the environment variables

of Redis host and Redis port.

The multi-server needs to know the same thing,

and the multi-server also needs to have

a set of environment variables

to tell it how to connect to Postgres as well.

So in this section and the next couple,

we're going to describe and discuss

how we set up environment variables in Kubernetes.

All right.

Now in this diagram, I took the same environment variables

and I did a little bit of color coding.

So first off, let's talk about the meaning

behind the yellow color inside of here.

So this, this, this, and this, all the yellow ones

are going to essentially be consistent constant values.

And we don't have to do a lot of work to set these up

inside of our different deployment definitions.

All the yellow ones

are gonna be pretty darn straightforward.

Now there's two red ones on here as well.

So that two host values.

These are also going to be constant values

that don't do not need to change over time

or anything like that.

But I want to remind you about the purpose

of the host environment variables.

These are essentially URLs of sorts

that are going to tell multi-worker and multi-server

how to connect to Redis and Postgres in the first place.

In other words, how it can actually reach out

to the running Postgres server or the running Redis server

and make a connection to them.

So with that in mind, I want to remind you

about the kind of overall architecture of our application.

Remember that we've got our Redis pod right here

and Postgres pod right here.

And we had said that the multi-worker and the multi-server

are going to connect to both those different pods

through the use of that ClusterIP service.

So we need to understand how we can tell multi-worker

to connect over to this Redis pod somehow

through this ClusterIP service.

And we're going to do this by providing a value

to the Redis host environment variable

and the PG host environment variable as well.

So let me just tell you,

let's get down to basics here.

It's a real straightforward thing.

Essentially all we have to do is say,

"Okay, we wanna form a connection from our deployment"

"of multi worker over to the Redis pod'"

"that is being managed"

"by the cluster IP service."

So all we have to do is provide a host name

of the name of that cluster IP service.

If you open up your code editor and find the cluster

client cluster...

No, not that one.

Where is it?

There it is.

Redis Cluster IP service right here.

Remember, we had given it a name of

Redis cluster IP service.

That's gonna be the host name.

So you can kind of imagine that we're going to connect

to http://redisclusterIPservice.

Now, there's not going to actually be the HTTP on there

I just put it on there to make you understand

that this is kind of like a URL.

And to connect from one pod over to another,

we just provide the name of that CLusterIP.

All right, so that's not too bad.

So in total, all the first set of environment variables,

like all these up here,

they're going to essentially be constant values.

And we just have to go around

to our different pod deployment configurations

and add in a little bit more config to say,

"Hey here's an environment variable"

"of like, Redis host and here's the value for it."

Same thing for the port

same thing for user, everything straight down.

Now the one value in this entire list here

that's going to kind of stick out

like a sore thumb

that we're going to do a little bit

of additional work on is going to be the PG password.

So PG password is a password

to our Postgres database.

And without a doubt,

we probably do not want to stick this as a constant value

or just in plain text inside of our Kubernetes config files.

And so we're gonna do a little bit more extra work

above and beyond for the PG password

and we're going to learn how to manage secrets

or secret variables inside of our Kubernetes cluster.

But we're gonna first do all the setup

for all these other ones

and then we'll tackle the PPG password at the very end.

So let's take a quick break right here.

When we come back to the next section

we're gonna start setting up our environment variables

for the multi worker and multi-server.
