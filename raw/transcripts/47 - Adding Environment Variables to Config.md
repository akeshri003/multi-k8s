![[Screenshot 2026-04-20 at 11.04.40 AM.png]]
![[Screenshot 2026-04-20 at 11.07.32 AM.png]]

-: In this section, we're going to update

our Config files for the multi-server

and the multi-worker and add all these different

environment variables.

Again, we're going to leave off the PGPASSWORD

for right now, we're not gonna

worry about that one just yet,

we're going to come back to it at the very end.

All right, so, we're going to first open up

our multi-worker config file,

the deployment for it,

and we're going to add in a little bit

of configuration to that.

So, inside of my code editor,

I will find the Worker Deployment file.

And inside of here, I'm going to scroll down

to the pod template. So here is my template section.

And then I'm going to find the container definition,

and we're going to add on a set of environment variables

that are going to be passed into this container

when it is created inside of the pod.

And so, on the container definition, I'll add on

and ".env", environment version .env key.

".Env" is short for environment variables.

And we're going to designate a couple here.

So, .env is going to receive an array,

so I'm going to put in a dash like so.

Then, for every entry inside this ray,

we're going to have a name and a value property.

And, I bet you can guess what these are going to do.

"Name" is going to be the name of the environment variable,

and then "Value" is going to be the value for it.

So, for our first named environment variable

that we're going to pass into the worker,

we'll pass in our Redis host.

And again, this is going to be some type of reference

telling our multi-worker container

how it can reach out and connect to Redis.

So, what we want to put in here,

is the name of the cluster IP service

that the worker needs to connect to

to get at the Redis pod.

And again, in this case, it's going to be

Redis Cluster IP Service.

Because that's what we provided as the name

of that cluster when we put it together,

that cluster IP, when we put it together

inside of our Redis Cluster IP Service file.

So, this is the string right here.

That's what we care about.

So, for the value, I will put in

"Redis Cluster IP Service."

-: (typing)

-: And then, next up, we're going to also

designate the Redis port.

The port value here is going to be 6379,

that is the default value for the

Redis connection port. It's also what we wired up

inside of our Redis Cluster IP.

Service as both the opening port

and the target port in there as well.

So, this again, is going to be

a hard-coded value, we're just going to

throw it directly in here as 6379, like so.

Okay, so that's it for the worker deployment.

It's all the environment configuration we need.

So, I'm going to close that file.

And we're going to also open up our, where is it?

The server deployment, right there, as well.

So, in the server deployment, we're going to add in

a set of environment variables to the server.

So, I'm going to find the server container configuration.

And, inside of here,

we're going to do our .env block as well.

Now, this one is going to have significantly greater

number of environment variables.

So, we're going to do a little bit of typing here.

Let's see. We'll start off first

with a name of "Redis Host".

-: (typing)

-: The value, again, is going to be the name

of our Redis Cluster IP service, which was

Redis Cluster IP Service.

We'll do another array entry, so notice how I put

another dash right here.

This is going to be our Redis port.

We'll have a value of 6379 over here as well.

Next up, we'll do our PGUSER.

So, this is going to be our

default username for Postgres.

And, we just use the default of Postgres,

which is technically not good form,

not the best way to make use of Postgres here,

but totally fine for our purposes.

Next up, we'll do our PGHOST.

So the PGHOST is the connection string,

we're essentially telling multi-server

how to reach out to our Postgres instance.

And so very similar to what we just did with Redis,

we're gonna do the same thing with Postgres as well.

We're going to provide the name

of the Cluster IP service that is managing

access to the Postgres pod.

So, I'm going to open up the Postgres Cluster IP service

config file right here, I'll find the name.

And that's going to be the host name

that we want to attempt to connect to

to get access to that database.

So, for my PGHOSTs, I'm going to put in

Postgres Cluster IP service, like so.

All right, so what do we have next?

We have our PGPORT.

The value for that, we are using the default

of 5432, no changes there.

Remember, we also set up our Postgres Cluster IP service

to make that port available.

And I think probably the last one,

actually no I think we got two more here.

What do we have?

I just want to double check the diagram

and make sure I'm not missing any in here.

So, we got the port, I think it's the database, right?

Yeah. All right, so we're gonna do our PGDATABASE.

And again, we're using the default Postgres

database of Postgres.

Again, very much like the PGUSER,

using the default database, not quite the best way

of doing things, but for our purposes,

it is good enough.

All right, so I think that's it in terms of

the initial set of environment variables.

So let's take a quick pause right here.

When we come back to the next section,

we're gonna talk a little bit more about the PGPASSWORD

and we're going to make sure that when we put this

into a config file, we are not writing out

the password in plain text.

We're gonna make sure that we store it

in a much more secure fashion.

So quick pause and we'll take care of that

in the next video.