![[sudo required.png]]
![[global.png]]
![[Pasted Graphic.png]]
![[Pasted Graphic 1.png]]

-: In the last section we had a long discussion talking

about how we're going to uniquely tag each of our images.

And the first thing we need to do is add some code

to our Travis.yml file that's going

to determine the current git SHA for our repository

and export it as a environment variable so

that we can later on use it inside of our deploy file.

I'm gonna flip over to my Travis.yml file

and up at the top, right after our services section,

I'm gonna add on a new section called ENV.

And then inside there, I'm gonna specify global.

And inside of here we're gonna set up

some number of environment variables.

So the environment variable that you and I care about

is getting the current commit SHA and I wanna store

that as an environment variable called simply SHA

so that we can easily refer back to it in the future

inside of our deploy.sh file without having to run

that git rev-parse command again and again and again.

So inside of here I'll say git SHA,

actually well let's keep it simple.

We'll just do SHA.

So that's going to be dollar sign, parentheses,

git rev parse head.

So, this right here is going to determine

the current commit SHA

and assign it to a environment variable

inside of our Travis environment alone called SHA.

So we can now freely access this environment variable

inside the deploy.sh file.

Now, while we're inside of this

environment variable configuration block right here,

there is actually one other environment variable

that we need to specify.

This is totally unrelated to all the SHA stuff.

I just wanted to make sure we took care

of both these environment variables at the same time.

So the second environment variable that we're

going to put together is cloud SDK underscore core

disable prompts equals one like so.

So, cloud SDK, this entire thing right here is just going

to configure the Google Cloud CLI

and make sure that it does not try to display any prompts

that require user input.

So in other words, when you and I run this command,

G Cloud off activate service account, if this thing normally

presents some warning or something that says like,

"Are you sure you want to do this, press Y or N."

We don't wanna see that because you and I

don't have the ability to respond to that

in a Travis environment.

And so this right here is just gonna make sure

that the Google Cloud CLI does not try to show us

any prompts such as that.

Now this is kind of a tricky command to write out here

so please make sure that you spell everything

in this correctly and that everything

is capitalized inside there.

All right, so again, I apologize

for that kind of little sidetrack

but again, I just wanted to do

all these environment variables at the same time.

All right, so we're gonna flip back over

to our deploy.sh file and we're gonna start

to make some changes to this thing now to make sure

that we apply not only a latest tag

to all of our built images, but the git SHA as well.

All right, so I'm gonna first start with the client.

Now here's the dash T my docker ID multi client.

I'm going to append that latest, like so.

So this is just going to be a hundred percent clear

that we want to tag this image as the latest version.

After that, I'm then going to add on a second tag.

So this is where this file's

gonna get a little bit squirrely

and there's gonna be a lot of text moving around here.

So we're gonna say dash T, so a second tag,

your docker ID slash multi client,

colon and then dollar sign SHA like so.

So now in total docker build, tag one with the latest,

tag two with our SHA that we had defined

as an environment variable inside the Travis.yml file.

And then after the second tag,

we should still see the dash F for the docker file

and the build context on the end.

Okay, so we're gonna repeat that process twice again.

So, on multi-server, I'll add on colon latest

and then we'll do another dash T multi-server SHA.

So again, double check this line.

We've got tag one with latest and then tag two with a SHA.

So then finally we'll do latest on the multi worker,

multi worker SHA.

All right, so tag one and tag two.

Again, do make sure that we have client server worker,

so make sure you did not accidentally put like server

in there or anything like that.

We have client server worker, client server worker,

client server worker, client, server worker

all the way down.

Okay, so that's looking good.

Now the next thing we need to do is make sure

that we push these new tags off to docker hub as well.

So, unfortunately when you do this docker push right here,

we are pushing very specific tags as opposed to an image

and all the tags that it has.

So, we have to run docker push

not only for multi client latest

but also for multi client SHA as well.

So for each these different tags we have,

we're going to have two separate push commands.

So I'm gonna look at all the current pushes

we have right here

and I'm gonna put on latest to each of them

just to be a hundred percent explicit.

And then I'll do another set of push commands.

Now for this one, I am just gonna do a copy paste,

no harm in this case.

So, there's my copy paste for client server worker

and then I'll update each of these to be SHA,

SHA, and SHA, like so.

So, now the last thing we have to do is make sure

that we get a separate set image command for each

of our different deployments and we need to make sure

that we specify multi-server with our git SHA

on the end as well.

And so for the first set image right here,

I'm gonna go to the very end of the line.

Here's multi-server and I'll append on colon SHA, like so.

And then we're going to repeat this command twice again

for the multi client and the multi worker as well.

So I'll do cube CTL, set image deployments,

client deployment,

client is going to be my docker id, multi client,

I'll put the SHA in.

And then finally, we'll do the same thing as well

for cube CTL set image deployments, worker deployment,

and the worker image, or excuse me, the worker container

inside that deployment needs to use the image my docker id,

multi worker, colon, SHA.

Okay, so that's it.

That's all we have to do.

Now that's the entire deployment script here.

Now again, I gotta ask you,

please do one last quick triple check inside of here.

Make sure you've got client server worker,

client server worker, server client worker,

server client worker, server client worker,

all the way across.

No one of these lines should have like duplicated client

client in a row like so.

They're all completely separate images

that we're working with here.

Okay, so that's pretty much it for our deploy.sh file.

And I think that's pretty much it

for our Travis.email file, as well.

So we're just about ready to push this thing

off to GitHub and test out our deployment.

But before we do, there's one or two last quick things

we have to take care of inside of our Kubernetes cluster

on Google Cloud.
