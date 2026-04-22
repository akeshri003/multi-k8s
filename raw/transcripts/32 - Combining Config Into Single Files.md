-: When we first started working on

taking our multi container application

and importing it into the world of Kubernetes

a couple sections ago,

I had told you that we were going to create

one separate configuration file

for every object that you see in this diagram right here.

I think in total we had something like 11 or so.

So that means that we're going to have 11 separate files

sitting inside of this K8s directory,

and each one of those separate files

is going to create a separate piece or a separate object

that's going to be running inside of our overall cluster.

Now, at some point in time

it might seem like having to work with

all of these different configuration files,

might be a little bit overwhelming.

Everything that we've done up to this point

has very clearly said

that we're going to make a separate file for each object,

but if it feels like that's just too many files

for you to have to kind of manage and deal with,

I wanna tell you about one different way

in which you can organize

all these different configuration files.

So rather than putting a separate file

for every separate object we create,

we can actually combine different sets of configuration

down into a single file.

So as a alternative way of organizing your config files,

rather than saying, Okay, I need a file for cluster IP

and a file for deployment,

we actually can create one single file

that contains all the configuration

for both a cluster IP or a service

and the deployment that it is associated with.

There's no limit to the number of different pieces of config

that we can stick into a single file.

So you do not necessarily have to say,

Oh, this service is going to be in the same file

as this deployment.

We could just as easily say,

one single file is going to hold all the config

for both these services and both these deployments.

I'm gonna give you a very quick example

of how you would combine

these different config files together,

and then I'll tell you

why we are not doing that inside this course.

Okay, so let's say for example,

that we want to kind of co-locate

all the configuration for our deployments

and services that are tied to each one.

So in other words,

we might want to kind of consolidate

the server cluster IP service

and the server deployment config files

down to one single file.

So to do so, I could create a new file

inside my K8s directory

and I'll call it something like, I don't know,

server config or something like that.

So serverconfig.yaml.

Then inside of here,

I'm going to go back over to my deployment for these server.

I'm gonna copy this entire thing

and then I'll paste it

inside of the server config file that I just created.

And then likewise,

I'm gonna go back over to the cluster IP service

for the server that we just put together a second ago.

I'll copy this entire thing

and I'll pull it over to the serverconfig.yaml file.

And then down at the very bottom here,

I'm going to put down ///,

or not slashes but dashes I suppose.

So three dashes in a row like so.

And then after that, I can paste in all the configuration

for the other object that I'm trying to create.

So essentially,

anytime that you want to kind of condense down

all this config

and put it to multiple objects worth of configuration

inside of a single file,

you're just gonna paste everything inside of here

and separate each one with the --- like so.

Now for some of you watching this video,

you might think, Oh, that's great.

I can make a single file that houses everything

that's related to this piece of my application right here.

But others of you might be thinking,

Well, how would I ever know

where the configuration for this service is located?

I would have to very actively understand

that every service is tied to the related deployment.

Now, that's really a question of preference.

You don't have to co-locate all this configuration.

I personally think that it makes a ton of sense

to separate all the configuration out into separate files

as we are doing right now,

because it very clearly tells you

how many different objects exist in your entire cluster,

and the naming scheme that we're using right now

also makes it extremely clear

where the configuration for any given object can be found.

For example, if you put this diagram

or a diagram like this right here

inside the Read Me of your project

and another engineer comes and works on your project

and says, Oh, I need to like change a port

related to the cluster IP service for the multi client pod,

they could very easily look at

your list of config files right here and say,

Okay, client cluster IP service.

Well, of course, the configuration for that thing

is going to be inside this file.

And they instantly know where to look

to find the configuration for any given object.

If it wasn't for that,

if you had combined everything into kind of single files

or condense everything down,

they would have to understand

that this cluster IP service

and the deployment for the multi client pods

are in the same file.

And so you would have to have some naming terminology

for all of your config files that makes it really clear

that like some particular file

contains all the configuration for the multi client pods,

its deployment and the service related to it as well.

And so it's a little bit harder

to come up with a naming scheme or a naming convention

that makes it really clear that,

hey, everything related to multi client

is inside of like XYZ file.

Now, that's just my take.

As I said, half of you might think that

combining this stuff down into a single file

makes a lot of sense

and the other half of you might think,

No, that doesn't make any sense at all.

I wanna do everything inside of separate files.

So like I said,

I prefer to keep everything in separate files,

but for you, totally up to you.

Of course, throughout the rest of this course

I really recommend that you follow the same strategy as I

and use a separate config file for each separate object.

Okay, so that's my spiel on combining together config.

Now I am going to delete

the server config YAML file I just created

because I had only made that as a very quick example

to show you how to combine stuff together.

So I'm gonna make sure that I delete that file

and I'm now left with just four configuration files,

the two client ones and the two server ones.

All right, So let's take a quick pause right here

and continue in the next section

with our next piece of configuration.