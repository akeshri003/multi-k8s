
![[Screenshot 2026-04-22 at 4.02.46 PM.png]]
![[Screenshot 2026-04-22 at 4.06.25 PM.png]]
![[bbug the app knowing the exact code that is.png]]

-: In the last section, we got a reminder

of something that we covered much earlier in the course.

Remember, any time that we set

an image manually on a deployment,

we have to make sure

that we use some image name or some tag on it

that is unique and different

than what the deployment is currently running.

If it is not different,

then the deployment it's just gonna say,

"Oh, no changes. I do not need to update myself."

So in this section, we're gonna figure out

how we're going to work around this issue

without having to always manually specify a version,

as we did previously.

Remember, when we initially discussed this problem

and we ran this kubectl set image command,

we had manually set on a version number

of something like version two or something like that.

And so of course, we don't want to have

to manually set version two or some text on here,

we want to have this be something

that is done automatically.

So, let's talk about how we're going to do this.

All right, here we go.

So, I took the same command

that we're just looking at a second ago,

the docker build command,

but put it on multiple lines here

just so you can read it a little bit more easily.

So when we run the docker build command,

we're going to apply two separate tags

to that image that gets created.

So it's the same image,

it's just gonna have two tags applied to it.

The first tag that we're going to apply

is the same one that we're already applying,

which is the multi-client latest.

Now to be precise, the current version of our deploy file

does not have latest on there,

we are going to go back and add it on

and we'll talk about why that's gonna be on there

in just a second.

Now more importantly,

to get some unique version identifier,

we're gonna add on the second tag of your docker ID,

multi-client and then a Git SHA.

Now, just in case you're not familiar

with what a Git SHA is,

let me give you a very quick demonstration.

If you flip over to your terminal right now

and are inside the complex directory,

remember that we have set up a Git repository

to track all the changes we make

to files inside this folder.

Anytime we make a set of changes

and then want to send those changes up to GitHub,

we have to create a commit with every commit.

With every commit that we create,

a unique identifying token, referred to as the SHA,

is generated.

And that SHA is essentially identifying

that current set of changes inside of our code base.

So to print out the SHA

that we are currently working with right now,

you can run git rev-parse HEAD like so,

and that's gonna print out the current SHA

for the latest commit that I put together.

We can also do a Git log

and that will print out all the different SHAs

for all of the different commits

that we have created over time.

So, we can essentially imagine that this SHA right here

is a absolutely unique identifying token

that's always going to be different

and it identifies the state of our code base

at a very particular point in time.

And so, it makes it absolutely perfect

to use as a version identifier for our images.

So, we're going to put the Git SHA

into our tag for the image.

So, when we get a image out of this thing,

we're going to add the two separate tags,

both multi-client latest

and multi-client followed by a SHA.

Now, when I put the dollar sign SHA right there,

I don't mean to say we're going to literally say,

dollar sign SHA.

I mean to say it's gonna look

a little something like this.

Like so...

So essentially, dollar sign Git SHA appear

means that we're going to determine the current SHA

and append it on to the end of the tag.

So when we eventually get a tag out of this thing,

it's gonna look something like this right here.

So then at some point in time in the future,

when we try to set our image on our deployment,

we'll be able to provide a image name

with a version tag on it of something like that.

And the deployment of course

is going to see that unique token right there

and it's gonna say,

"Oh, this is some new version of the image.

I'm definitely gonna update myself

and create a new set of pods using this new image."

So, that's the idea,

we're going to use our Git SHA here.

Now, besides just being a way

to uniquely identify the current image

or the current version of the image that we're using,

there's actually another benefit or two

around using the Git SHA.

I want you to imagine for a second

that we go through this deployment process,

and maybe we deploy our app several times,

and then at some point in time in the future,

we go and visit our website

and we realize that it's completely broken

and it's just not working the way we expect.

And so of course, we would want

to immediately do some debugging.

Probably, we would want to figure out exactly

what code we are running inside of our Kubernetes cluster

or essentially what exact state of the code base

is inside of all the different images

that are inside the cluster.

So, in order to debug a broken cluster

or something running some really bad images,

here's what we could do.

We would say, "Oh, no, you know, our app is breaking,

we need to figure out the exact state of the code base

that the deployments are running."

In other words, "Give me the exact version

of that client image

so that we can debug it locally."

So we could run a command to figure out exactly

what image the deployment is running.

So it might say,

"Okay, we're running multi-client version A3BA,

whatever it is.

So then back inside of our local repository,

we can run Git checkout with that commit SHA

and that will revert our code base back to the old version

or the version of code

that is currently running in our deployment.

So, then we could then debug our application locally ,

knowing that we are debugging the exact same code

that is running inside of our production environment.

So when you start using these Git SHAs

as a way of tracking your deployments,

you do get some really good benefits.

Now, the last thing I wanna mention here very quickly,

you might be curious why we are still going to apply

the latest tag to our image that we are building here.

So yes, we are going to put the two separate tags on here.

But why are we still going to put on latest?

Well, here's why.

I want you to imagine that we run this command,

you know, with latest and Git SHA,

we build our images,

we push them up docker hub whatever happens,

and then at some point in time,

maybe some new engineer comes onto your project.

And so, some new engineer comes along,

they clone your repository

and then they run the command, kubectl apply K8s.

So they're gonna take all the different config files

inside the K8s directory and apply them to their local copy

or their local mini cube cluster for Kubernetes.

Now the deployment files...

So, we have specify an implicit latest tag.

And what I mean by implicit,

is that inside of all of our different config files

for our deployments right now,

if we open up, say, the client deployment,

we have an image right here of multi-client.

So again, the implicit understanding

or what Kubernetes is going to do automatically

is pull down the latest version.

We don't have to specify latest,

it's just going to do that automatically.

So when this engineer,

this brand new engineer runs kubectl apply K8s,

and all of our deployment files

specify essentially the latest tag,

then we're going to automatically

be giving this new engineer

the latest version of each of those images

and that engineer does not have to worry

about somehow getting the current commit SHA

and specifying that particular image

for what they want to apply on their deployment.

That's why we are doing both here.

We're doing the SHA, to make sure

that we can correctly update stuff in production

and we're inside of our cluster

and we are still doing the latest

to make sure that if we ever have reclone

or rebuild our cluster at some point in time,

we always know that the latest tag image

is truly the latest version of our image.

And we don't have to go and try to track down,

like what our current SHA is or anything like that.

Okay, so I know this has been a long discussion

about how we're doing all this build stuff.

I think we have a good idea of what's going on now.

So, let's take a quick pause right here.

When we come back to the next section,

we're gonna start to update our Travis YAML file

and the deploy script

to incorporate this idea of using our Git SHA.
