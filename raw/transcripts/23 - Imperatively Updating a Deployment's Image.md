![[Screenshot 2026-04-19 at 5.42.59 PM.png]]
![[Screenshot 2026-04-19 at 5.47.49 PM.png]]
![[Screenshot 2026-04-19 at 5.49.33 PM.png]]

  
![Lecture thumbnail](https://mp4-cdn77.udemycdn.com/2018-08-22_01-50-10-42aa75dba71284f512c94dd43d23184b/2/thumb-sprites.jpg?secure=GyrjImLcWhiYuGAS-ODQ4g%3D%3D%2C1776616924)

5:52 / 7:24

-: In this section, we're going to walk through

the update process we described

at the end of the last section.

So here's what we need to do.

We're going to first tag our image

with a distinct version number,

and then we're gonna push that new version up to Docker Hub.

After that, we're going to run

a rather long kubectl command

that's going to forcibly update our deployment

and tell it to use this new version of the image

that we've have pushed off to Docker Hub.

So let's get to it.

Step number one is going to be to tag our image.

So to get started, I'm gonna open up my terminal,

and I'm gonna create a second terminal window

and then change on over to that complex client directory.

So I'll go into complex,

and I'll change into the client folder.

So here's my client project.

We have already built an image out of this folder,

and we have not made any changes to our project since then.

So we could technically do a Docker tag here

and tag the existing image that we already built.

But instead, just to show you the entire flow,

I'm gonna build the entire image from scratch again.

So I'll run docker build and I'll tag this thing

with my Docker ID slash multi-client,

and I'll make sure I get a colon on there.

And then we have to put some unique token

on the other side of the colon.

And so for me, as a version number,

I'll just do like, I don't know,

version five or something,

anything you wanna do.

And then after that I'll put down a period

to specify the build context.

Okay, so I'll run that command.

The image is rebuilt and it's now tagged

with your Docker ID slash multi-client colon V5.

So now we need to make sure

that we push this updated image over to Docker Hub.

So I'll copy the entire tag right there,

and I'll do a docker push and copy the tag in.

All right, so just like that,

we now have a new tagged image over on Docker Hub.

So now we need to run a kubectl command,

that's going to force our deployment

to use that new image version.

Back inside my terminal,

I'm gonna delete the second terminal window that I opened,

and so I'm left just with the original window

that's based on the simple K8s directory.

So inside of here, we're gonna run a rather long command.

I got a quick diagram to tell you a little bit about

what we're going to be writing out.

All right, so here it is.

Yes, it's rather long.

So we're going to use kubectl,

and we're going to use the set command.

We use the set command to update a property

on one of our objects that exist inside of our cluster.

The specific property that you and I want to update

is the image property.

So the image property that is tied

to our single container right here.

After that, we'll specify the type of the object

that we want to update.

So in our case, we are not updating a container,

we're not updating a pod,

we are updating a deployment.

That's what we're updating.

So we're gonna specify

a deployment slash the name of our deployment,

which for you and me

is client dash deployment, right there.

After that, we'll then specify the container name.

So remember, a deployment creates pods,

and inside of a pod we can have many different containers.

Our specific pod template right here,

only has one container,

but we could very easily have other containers

inside of here as well.

And all those other containers

would have their own image property.

So we need to make sure that we specify

which of all of our containers we want to update.

And we specify the container that we want to update

by specifying the name.

So we're going to say the container name,

and for us the container name is client.

We'll then do an equal sign,

and then the full image that we want to use.

So it'll be your Docker ID slash multi-client,

and then a colon, and then the version number

that you use to tag the image with.

So that's the entire command.

Let's give it a shot.

Back inside my terminal,

I'm gonna run kubectl, set image,

then we're gonna do our object type,

which is a deployment slash the object name

which is client dash deployment,

and we'll put in a space.

so there is a space there.

Notice how it wrapped the line for me

but there definitely without a doubt, is a space there.

And I'll specify the container that I care about,

which is client.

And then the update that we're going to make,

is to say that we now want to use

our Docker ID slash multi-client.

And then the version that we want to use

is whatever version you just used a second ago

when we rebuilt our image.

And so for me it is V5.

All right, so that's it.

So we're gonna run this command

and we get that our image has been updated.

So now we can do a kubectl get pods,

and you'll notice how we have a single pod here.

And most importantly, it has an age of around 5, 6, 7

or however many seconds,

which definitely means without a doubt,

that our pod was just recreated by our deployment.

So now we get to test this out inside of our browser

to make sure that the update actually went live.

So remember, to access our running container,

we need our minikube IP.

So there's the IP address.

And then remember, the port for our application

or for this particular pod is 31515,

as specified inside of our client node port service file.

So inside my browser, I'll open up a new tab,

I'll put in my IP address, and I'll go to 31515.

And then once here, I should see Fib calculator version two.

Now really quick, if you don't see version two,

do not panic.

The react application we put together

has some caching built into it.

So if you do not see version two over here,

then the first thing I want you to do,

is give it a couple seconds and then do a quick refresh.

Failing that, try opening up your Chrome console,

expand this tab on network, right here.

You can select disabled cache

and then try refreshing again.

And then if even that doesn't work,

you can always run the set,

where's that command?

The big set image command here again.

Try running that a second time.

And then after all that,

you should be able to eventually refresh this thing

and se version two appear.

Like I said, sometimes it does take a second or two

for it to actually pop up the update inside of here,

but you should eventually see it go live.

All right, so that's it.

That's how we update

or how we tell a deployment

that we wanted to use the newer version of an image.

Now, I think you'll agree with me

that this entire process

is definitely a little bit of a pain

because we have to rebuild the image,

we have to apply a unique tag version on it,

and then we have to run that command.

But when we eventually set up our entire cluster

to be deployed off

to either Amazon Web Services or Google Cloud,

you and I are gonna write out a big long script

to facilitate our deployment.

And inside that script,

we're going to put all of this tagging logic,

and all the version tagging,

and all the kubectl set image command stuff as well.

And so when we eventually move over

to a production environment,

all this versioning stuff

is going to be completely automated for us

and we're not gonna have to do any additional work.

So it's really just kind of understanding

what's going on behind the scenes.

That's the challenging part.

Once you understand what's happening from then on out,

life gets pretty darn easy.

Okay, so that's pretty much it.

Let's take a pause right here

and we'll continue in the next section.