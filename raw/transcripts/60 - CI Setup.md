![[Screenshot 2026-04-20 at 4.42.10 PM.png]]

-: In this section, we're gonna start talking about

the travis.eml file that we're going to put together

to build all of our different images

and then eventually deploy our application

to our Kubernetes cluster.

Now, unfortunately, all the travis.eml files

we've put together before we're all kind of preparation

for the one that we're going to put together now,

because by a large margin

this is going to be the most complicated travis.eml file

that we're going to put together.

Lot of stuff is going to be going on inside of here,

lot of different configuration.

So I've got a little flow diagram to give us an idea

of what we're going to be doing inside of our config file

to eventually deploy our application.

So everything is gonna start inside of our config file

by installing a Google Cloud SDK.

Remember, the entire purpose of Travis

as we are using it, is to not only test our code

but then to also deploy our application

after our test runs successfully.

And so we need to make sure

that Travis has the ability to somehow reach out

to our Kubernetes cluster and make changes to it,

or essentially run a series of configuration files

and apply them to our cluster.

So to do so, we're going to install a Google Cloud SDK.

This is a CLI that is going to allow us to

essentially remotely interact with

and configure our Kubernetes cluster,

most notably by applying different config files.

Now, unfortunately this SDK does not come

kind of preconfigured with Travis.

In other words, we have to actually download

and install the SDK every time that we run our Travis built.

So after we install this thing, after we download

and install it, we then have to configure the SDK

with some information from our Google Cloud account.

Essentially, we're going to authorize this CLI

to make changes to our Google Cloud account

and more specifically,

the Kubernetes cluster that we just created.

So that's kind of a little bit of preamble,

some stuff that we're gonna do at the very start

of the script and get this CLI ready to then later on

deploy our application towards the end of the process.

After that, we're then going to start to go through

with some steps that are very similar to what we had done

previously on our other different Travis builds.

So we're going to log into the Docker CLI.

We're going to build the test version

of the multi-client image,

and we're going to use that thing to run our tests.

Remember, we don't really care about the tests

that are inside of that multi-client image.

I just want you to have a good example of

how you would run some tests inside of your application

so that when you actually go and apply all this knowledge

to your own personal project, you understand like,

okay, at this phase right here, I'm going to run a command

that's going to execute my test.

Again, we don't really care about the test

in our particular case.

After that, if all the tests run successfully

we're going to run a script.

So this will be a separate script

outside of our travis.eml file

that's going to attempt to deploy the newest images.

Inside that script,

we're going to build all of our different images

so that's the multi-client, multi-server, and multi worker,

and then we're gonna push each one off to Docker hub.

After that, we're then going to apply

all the different config files inside of our K8s directory.

So specifically, everything inside of here.

We're just going to apply the entire directory.

Now, the benefit to that is that

if at some point in time you and I decide,

oh, hey, we need a new deployment

or we need a new service, or whatever it might be,

all we have to do is add a new file

to the K8s directory

and then push our code up to GitHub.

After Travis builds our code, it's then going to apply

all the config files in that K8s folder.

And so essentially our Kubernetes cluster

is always going to be 100% in sync

with our GitHub repository.

Any changes that we make to our config files

in the K8s directory are always going to be applied

to our Kubernetes cluster in Google Cloud

as soon as we push our code off to Travis.

So overall, it's actually a pretty cool system.

Now, after we apply those configs,

we have to do one other little step.

Remember, we had a lecture a while ago

where I showed you how it was kind of a pain in the rear

to get a deployment to use the latest version of an image.

We saw that if we just tried to reapply

a deployment config file that did not somehow magically

get the deployment to automatically go out to Docker hub

and check to see if a new version was available.

And so as a workaround, I had showed you that

very special command that very imperatively

set the image version that a deployment was supposed to use,

and that's how we're going to eventually

get our deployments to make use of the new images

that we built during this step right here.

So in addition to applying all those configs,

we're also going to make sure that we do some

imperative command to update the versions of each image

that each of our deployments is making use of.

Okay, so a lot of knowledge here.

Like I said, this is going to be a really intense

travis.eml file, and there's even a couple steps

during this process that I didn't actually write

into this diagram just because it would make the diagram

a little bit crazy.

So a lot of stuff going on, a lot of stuff to talk about.

So let's take a pause right here.

In the next section, we're gonna create our config file

and start by installing the Google Cloud SDK.

So quick pause and I'll see you in just a minute.

PLEASE ALSO ADD TO THE NOTES, THE STEPS FOR GITHUB ACTIONS BASED CI, CAUSE THAT IS WHAT I WOULD ACTUALLY BE USING FOR MY TUTORIAL
