
![[Screenshot 2026-04-22 at 3.57.31 PM.png]]
![[Screenshot 2026-04-22 at 3.57.43 PM.png]]
![[Screenshot 2026-04-22 at 3.58.48 PM.png]]![[Screenshot 2026-04-22 at 3.59.41 PM.png]]
![[Screenshot 2026-04-22 at 3.59.17 PM.png]]
![[Screenshot 2026-04-22 at 3.59.57 PM.png]]
  

-: In the last section, we created a deploy dot SH file.

So inside this file we have to do our image building

apply some configs and imperatively, set the latest images.

Now, I've repeated this series of three steps right here

like five or six times

and I'm sure you're tired of me saying it.

The reason that I keep talking

about these three steps right here is

that it's actually gonna be a little bit more challenging

than it might seem at first glance.

So we're gonna do kind of a initial take

on these steps right here.

And then as we start to go through a couple of these steps

we're gonna very quickly realize

that there's a little bit of an issue

with some of the config that we'll put together

for these steps that you would think would be reasonable.

So in other words, we're gonna write out

what's gonna seem like the right code

or the right commands, but at the end of the day,

something's gonna be just a little bit off.

All right, so let's get to it.

Inside my deploy dot sh file

we're gonna first write out a series

of commands to build each of our different images

and then push them each off to Docker hub.

So I'm going to do a Docker build, I'm going to tag it.

I'll put in my Docker ID.

We'll do our multi client first.

I'll specify the Docker file location,

which is dot slash client Docker file.

And then I will also specify my build context

of dot slash client.

Now I'm gonna zoom out for just a second so you

can see that entire line.

Okay, so there it is in entirety.

Now I'm going to do the same thing twice again.

Once for our server image and once for the worker.

I encourage you to not do a copy paste here

because if you do a copy paste

incredibly likely that you might forget to change

one of the mentions of client inside of here.

All right, so we'll do a Docker build

Docker ID,

multi-server,

server, Docker file, and server.

So now at this point I want you to double check

three locations.

One, two, three.

Those should all say server.

Should not see any mention of client on the second line.

And they'll do the same thing a third time around.

So I'll say Docker bill dash T,

worker,

get the worker Docker file, and then finally

specify the build context of worker as well.

And so now again I want you to triple check

multi worker, worker and worker.

Cool. So that looks good.

Okay, so that is us building our images.

So now the next thing we have to do

is take to those images and push them off to Docker hub.

Now we've already logged into Docker through

our Travis dot YAML file.

The Docker command that we're using right here

is the same Docker that we're kind of configuring

back inside the Travis dot YAML file with the

Docker login command, wherever we put that.

Here it is right here.

So we don't have to log in again or anything like that.

We can just freely take each of these images

and push them off to Docker hub.

So I'll do a Docker push via Docker ID multi client

and then I'm going to repeat the process two more times

and let me zoom in now

since we're looking at a very shorter line here.

So I'll do Docker ID,

multi-server and finally,

multi worker like so.

Okay. So yeah, it still seems

like everything is looking pretty good.

Let's go back over to our diagram over here.

So the next step after building

and pushing those images is to take all the configs

in the K eight directory and apply them.

Now remember inside the Travis dot YAML file

we already configured Google Cloud.

And Google Cloud is in charge of our kubectl command

inside of this Travis environment.

So we don't have to do any more configuration of kubectl.

We can just write kubectl commands in the exact same fashion

as though we were running them on our own local machine.

And so to take all of the different config files inside

of our K eights directory

we'll simply do kubectl, apply, dash F, K eights.

So take everything inside that K eights directory

apply all those config files.

Okay, so now the last step we have to go through

imperatively set the latest image on each deployment.

So this is where things are gonna start to

get really interesting, and we're going to very

quickly see that there's

a little bit of an issue with what we are doing here.

So remember how we set a image forcibly

or imperatively on a deployment?

We do it by writing out kubectl set image.

Then we'd reference the object type that we want to update.

Then the name of our deployment,

which in our case for the server is server deployment.

And we can get the name of that from our K eights directory.

Here's the server deployment file right here.

And we had given it a name of server deployment.

So that looks good.

So we're saying deployments server deployment

and then we select the server container

and we tell it to use the image

my Docker ID slash multi server, like so.

All right.

So I wanna look at this configuration

that we just put together

and notice how we're not gonna do the last two

or the other two deployments in here just yet.

Cause I wanna point out something that is not quite correct

with how we've written out this command right here

and the entire series of commands inside this file.

Don't worry

I'm not saying we have to rewrite the entire file

I'm just saying that there's one

or two little things inside of here that we need to tweak.

So I want you to think about what's going on inside

of that deploy file right now.

At the very top, we're essentially saying,

Okay let's do our Docker build

and then we tag our image,

like in this case,

it's the multi client with just the name

whatever my Docker ID is slash multi client.

And so the results of this entire command right here

is an image with the command

or something with the tag Docker ID multi client.

And remember, anytime that we build an image

if we do not specify a tag, the implicit understanding

is that it gets automatically a tag of latest applied to it.

In fact, you can actually double check that right now

if you check out your Docker hub account.

So if you go to hub.docker.com

and look at your multi client image, look at tags,

and you'll see inside of here, here's latest.

So I've got some other versions.

So we had put together previously, but essentially

right now the latest copy is tagged as latest.

That is the implicit tag that is assigned

to any image that I build and push off to Docker hub.

So what's that mean?

Well, it means that later on when we run Docker

blah blah or kubectl, set image, blah, blah, blah

and then specified the multi-client image that deployment,

and this is something that we ran into previously remember,

that deployment looks at the image that is specified

it sees Docker ID slash multi-client

and then maybe we add on old and latest here,

maybe we don't.

Doesn't really matter either way.

Essentially the deployment is gonna look at that image name

and it's gonna say,

Oh I was already running the latest image

so I don't have to make any change whatsoever.

Remember we spoke about this at great length

earlier on inside the course.

We had spoken about the set image command.

We had spoken about how there's a couple of different ways

of somehow telling a deployment to update the version

of an image that it uses.

And we had eventually landed on saying

that we were gonna use a imperative command to

update our deployment.

So we had said

that the I entire flow was gonna be something like this.

We were going to change our, well,

this was a very particular workflow right here

but we had essentially said

that we were going to update the multi-client image

and we were then going to very importantly,

tag the image with a unique version number

and push it up to Docker hub,

so that sometime in the future when we used Cube CTL to

set the image that the deployment used

we could specify a very particular version of that image.

So that the deployment would say,

Oh, okay I see you've applied some new version here

or it's a image name that is different

than the one that I'm currently running.

So an update is required.

So essentially the issue

with all the deployment commands we've put in here so far

is that we're kind of operating under the assumption

that all these images are using that latest tag.

And as soon as we set an image

with the latest tag on our deployment,

the deployment's gonna say,

I'm already running latest, no change required.

All right, so now that we remember this entire situation

because we did cover it quite a bit ago inside this course

let's take a quick pause when we come back

to the next section, we're gonna talk about how

we can get some version numbers to be automatically applied

to all of our different image tags,

and how we can essentially solve this entire issue

with making sure that our deployments

really get the latest version of these images.

So quick pause and I'll see you in just a minute.