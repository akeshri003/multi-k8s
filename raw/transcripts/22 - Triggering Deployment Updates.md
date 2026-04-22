![[Screenshot 2026-04-19 at 5.30.46 PM.png]]

![[Screenshot 2026-04-19 at 5.34.45 PM.png]]

![[Screenshot 2026-04-19 at 5.37.28 PM.png]]

 -: In the last section, we updated our multi-line image

and then we pushed the result off to Docker Hub.

We're now going to move on to step three

which is to somehow convince our deployment to recreate

or otherwise update all of our pods

with the latest version of that multi-client image.

Now, unfortunately, step three right here

is very challenging and there's not

a very good solution around it.

Now that might be very surprising,

but I just wanna be very clear with you,

in the world of Kubernetes,

convincing a deployment to recreate our pods

with the latest version of an image

is not the easiest thing in the world to do.

So inside this section, we're going to take a look

at why this is challenging, and then we're going to look

at a couple of different ways to solve this problem.

Now, before we start talking about why this is challenging

or any of the solutions, I wanna very quickly mention to you

that on the GitHub page for the Kubernetes repo,

you can go to the issues board

and reference issue number 33664.

To get to this issue in particular,

you can just type that issue number directly

into your address bar.

This is a entire issue with a ton of different conversation

going on here, talking about how challenging it is

and suggesting different ways of convincing

your deployment file to somehow update all the pods

that it is managing.

So I do recommend maybe taking a quick glance

at this issue thread and getting a better idea

of some of the root causes here

and some of the different ways of solving the problem.

All right, so let's take a look first

at why this is so challenging to get a deployment

to update the image that is being used by all the pods

that it manages.

I wanna first remind you that in order to update

any object inside of our Kubernetes cluster,

we make a change to our deployment file,

and then we send the deployment file off

to the Kubectl command line tool with something

that looks like this right here, right?

We do the Kubectl apply.

Now, the issue that we have right now

is that inside of our deployment file

there's nothing inside of here that says,

Hey, I want to use some particular version

of our multi-client image.

So when we send an update to multi-client off to Docker Hub,

when we make our updated version

of that image and we send it off,

there's nothing that we can kind of turn back around

to this file with and say, Hey, I want you to now use

like the latest image.

Now, the reason that's such a big deal

is that if we try to take an unchanged configuration file

and apply it with Kubectl

if there's no changes in the configuration file

then Kubectl apply is just

going to flat out reject the file.

Let's try that out right now.

Remember, we've already applied this file

and we've made no changes to it since we last authored it,

or excuse me, since we last applied it.

So back at my command line

I'll do a Kubectl apply, dash-F,

client deployment, dot-YAML.

and then when I run this command

it's gonna tell us that the file is unchanged.

And so the entire file, the entire apply command

is just flat out rejected because the config

that we're trying to upload is identical

to the last config we put in place.

Now when you put this file up, it's unchanged.

That means totally rejected.

So there's nothing inside that process that says,

Oh we should probably go and see if a new version

of this image is available.

That does not occur.

At no point when you try to re-upload

or reapply that config file, it does not try to see

if a new version of this image is available.

And so the entire apply command is just flat out rejected.

That's why this is such a big issue.

Okay, so with all that in mind, we're now gonna look

at a diagram that's gonna lay out three possible solutions

so we could use to get around this issue.

So none of these solutions are really that great.

I'll be honest with you, none of them are really that good.

They're kind of the best of a bad situation.

Okay, so here's the first option we can use

to get our deployment to update

with the latest version of that image.

We could manually delete all the pods

that are managed by that deployment.

Now, when we manually delete a pod

that is managed by a deployment,

the deployment is going to very quickly notice

that the pod is missing and it's going to attempt

to recreate it automatically.

So in other words, if we go over

to our terminal right now and do a Kubectl get pods,

and then attempt to delete this pod right here,

the deployment will very quickly notice

that it has a missing pod,

and the deployment will recreate this thing automatically.

So our hope there would be that when the pod gets recreated,

hopefully it would pull down the latest version

of that image, and so we could use that

as a means to recreate our pods with the latest image,

or excuse me, latest version of some particular image.

Now, why would we not do this?

Well, as you might guess, deleting pods seems

like a really, really silly situation,

or really silly solution.

There's so many things that could go wrong

with this approach.

In a production environment,

we could very easily accidentally delete the wrong set

of pods, totally possible.

So if we delete the wrong set of pods

who knows what type of bad situation we would be in.

In addition, if we manually delete all of our pods related

to one very particular aspect of our application,

let's imagine that maybe this is the pod

that serves up all of our web traffic.

And so if we delete all those pods,

for a very brief period of time,

any user trying to access our application

would be trying to get at a pod or get at a container

inside there that just doesn't exist,

and for a very brief window our users would essentially

not be able to access our application entirely.

And so everything about deleting these pods

just kind of seems like a bad idea.

Okay, so let's take a look at the second possible solution.

All right, so I wanna give you a quick reminder

on tagging of Docker images.

When we tag a image, we put down usually our Docker ID,

a slash, and then the repository or project name.

Now, one other thing that we can optionally put

in here is a colon and then a version for this image.

And so we could put in something like, you know, v1,

and then maybe the next time we build the image,

we would increment it to v2, or v3, or v4, or v5,

or whatever it is.

So as a possible solution to this problem,

we could decide to tag our images with real version numbers

and then specify that version inside the config file.

So in other words when we build our image back over here

inside of, you know run our Docker build command

and tag the image, we could put on a version number,

like v1, v2 v2, or v4, whatever it might be.

We then push that tag up to Docker Hub

and then inside of our config file,

we could append on that v1, v2, v3, v4, or whatever it is.

Now when we change the config file with a version number

like this, this is an actionable change to our config file.

So if I put on here v4 and then save the file

and then rerun that Kubectl apply command

that is detected as a very real change to our config file.

And so the config file would be accepted

by Kubectl apply because we are specifying

a completely different image version.

Our deployment would then use that new image version to

recreate and update all of the pauses that it manages.

So this would be one possible solution.

Now, the downside to this approach is

that it really adds in extra step to our deployment process

and it's definitely not a very friendly step.

Just think about what we would have to do.

We would e essentially have to make sure that anytime

that we build our images, we would have to append

on some version number over here, like, you know

1, 2, 3, 4, 5, whatever it is.

And then we would have to go over to our configuration file

and put the exact same version number

inside of here as well.

So we would have to remember the current version

and somehow insert that number inside of here.

And that's definitely a non-trivial step to add in here.

Now, one quick workaround to that

that you might imagine if you're, you know

a little bit savvy, you might think, okay, well

if it's such a pain to get the version number in here

maybe we could get like an environment variable or something

like that that would carry the version of that image,

so when we build the image,

we could store the version number in an environment variable

and then reference that environment variable

inside of our file.

So we could do something like, you know, client version.

Well, unfortunately, we are not allowed

to use environment variables inside of these config files.

So that solution kind of goes out the window

as being an easy workaround.

So specifying the version, it definitely would work,

but it adds in that extra step of having

to assign a version number when we build the image,

and then we also have to make sure that we somehow

inject that version number into the file.

We literally would have to inject the version number

through some type of like templating process.

So that definitely doesn't seem like it's

a very good version or very good solution as well.

All right, so now the last thing that we could do

is we could use an imperative command

to update the image version that the deployment should use.

So this is somewhat similar to the last step.

It's saying that when we build our image

we would still append on a version number

so like you know, v1, v2, v3, whatever it is.

But then rather than updating our config file

we could immediately run a command

after building our image that says to Kubernetes,

essentially like, Hey, Kubernetes,

like go find my client deployment

and update the version number, like, you know,

I don't know, I'm just making up this command here

update version to v1, or something like that.

Now, this is very similar to the previous step

in that we are still going to tag

our image with a version number,

but the downside to this approach

is that it's using an imperative command

and it's essentially completely skipping over

our config file and keeping our config file

up to date with the state of our cluster.

So all three of these steps seem like,

you know, bad solutions.

They don't really seem like they're the best things

in the world to solve this problem.

I think without a doubt,

the first solution over here is definitely very, very bad.

I think that this solution right here is definitely

a pain in the rear because we are making a change

to our config file and anytime that we're changing

our config file we would probably want

to do a git commit, right?

We would probably want to commit that new version.

Well, the downside to wanting to do that git commit

is that, remember, we traditionally only build

our versions with the, or build our images

with the Docker build command right here

when we are in a CI environment,

like off on Travis CI.

And so we're only on Travis CI when we've already

made a get commit.

So we're kind of talking about

like making a get commit to update the image

and then pushing this off to Travis,

getting the image built right here,

and then simultaneously taking that version number

and updating this file, and then committing this file too

at the same time while we're still on Travis.

Now, if that doesn't make sense, that's totally fine.

What I'm trying to say is that this step right here,

it definitely sounds like it's a good solution,

but in practice it ends up being a tremendous pain

in the rear.

So that kinda leaves us with solution three over here.

We're going to use an imperative command.

So after we rebuild an image, we are going to tag it

with a version number, and then we're going

to very specifically reach out to Kubectl

and tell it to update our deployment with the latest version

of the image that we just pushed up to Docker Hub.

Now, I know this uses a imperative command

and I just told you how we like to avoid these as much

as possible, but again, this is kind of

like the best of a bad situation

and I think that you'll see when we eventually

deploy our application to a production environment

this ends up being like a pretty reasonable solution.

It's not the best solution

but it's pretty reasonable when it really comes down to it.

Oh, okay.

So this has been a very long section.

I apologize for the length, but again,

I wanted to make sure it's really clear

that this is just a continuous not nice issue

to have to deal with, and I wanted to go over some

of these possible solutions because these three

in particular get outlined in this issue thread over here

if you decide to read this thing.

All right, so with all that in mind

let's take a quick pause right here.

We're gonna come back to the next section.

We're gonna learn how we can somehow tag our image

with a unique version number when we build it

and then make sure that we can run a command

that's going to tell our deployment to update itself

with the latest version number available.

So quick pause and we'll come back in the next section.