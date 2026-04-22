-: In the last section we finished up

all of our Postgres persistent volume claim stuff.

So the last thing we have to do

is apply it to our local cluster.

I'm gonna flip over to my terminal.

I'm gonna make sure I'm still inside

of my complex directory,

and then we'll do our kubectl apply -f.

And as usual, we're just gonna throw everything

inside of the K8s directory into the apply command.

So I'm gonna run that and we're gonna very quickly see

that we have created a new claim,

and that we have reconfigured the Postgres deployment.

So now we're going to get the status

of our deployment and all of its pods,

and just make sure that everything

is still working the way we expect.

So the first thing I'll do is a kubectl get pods.

We're going to see Postgres inside of here

and it looks like it just got restarted

and is now running, so that's good.

I'll do a kubectl get pv,

which stands for persistent volumes.

And this is gonna list out

all the different persistent volumes

that have been created inside of our application.

And on here we can see that we have a persistent volume

with the name of pvc, blah, blah, blah, blah, blah.

It has two gigabytes, this is the access mode,

and there's a couple other columns on here.

I'll probably have to zoom out really far.

Okay, on the very far right hand side,

you can see that it has a claim,

using our database persistent volume claim.

And the status of it is bound,

which means that it is currently in use.

We can now also do

a kubectl get pvc,

and that will list out all the different claims

that we have created as well.

So remember, the claim right here is an advertisement,

it is saying, "You can get this thing if you want to."

The persistent volume right here is saying,

"Here's an actual instance of storage

"that meets all the requirements that were laid out

"by the persistent volume claim that we made."

Okay, so that's pretty much it.

So hopefully now if we write any data to Postgres

and then for some reason kill that pod,

ideally it will not wipe out any of the data

that have been stored inside the database.

Now we're not gonna be able to test that yet,

because we have not yet configured Express

or our worker instances to actually save data into Postgres.

So that's the last thing

that we really need to take care of.

Let's take a quick pause right here,

and in the next section we're gonna figure out

how to add in some environment variables

to make sure that our server, and the worker,

can both connect to Postgres and Redis

to save some amount of data.

So quick pause and I'll see you in just a minute.