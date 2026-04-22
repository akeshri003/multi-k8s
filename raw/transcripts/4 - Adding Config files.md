
  
of steps that we're going to go through to

get our first container up and running on Kubernetes.

So the first thing we're going to do is double

check docker hub and just make sure

that our image is up there for the multi-client image.

All right, so to get started, I'm gonna open

up a new browser tab and I'll navigate to hub.docker.com.

And once over here, I'll look

at my list of repositories and just verify,

yep, my image is up here.

Stephen Grider \ multi-client.

This is the repository that contains the image

that we want to create a new container

out of on our local Kubernetes cluster.

Okay, that was easy enough.

So I'm gonna close that tab

and we're now gonna move on to our second task

which is to make a 1config file to create our container.

Now, to make this config file

we're going to create a new project directory,

we're going to open up our code editor

inside that project directory,

and then we're gonna start writing a little bit

of configuration into it.

All right, so I'm going to flip on over to my terminal.

You'll notice that I'm back inside of a

workspace directory of sorts.

So, I'm no longer inside of that complex folder

that we had been working in previously.

Out here, I'm gonna make a new folder called

Simple K8s.

The term K8s is a little abbreviation

for the word Kubernetes

and you'll very frequently see the word K8s

in place of Kubernetes in a lot, a lot of documentation.

The 8 in there is meant to represent the 8 letters

between K and S in the word Kubernetes.

Okay, so I'm gonna make that directory.

I'll then change into it

eventually.

There we go.

And then I'm gonna start up my code editor

inside that directory.

All right, there we go.

So, now inside of here

we're going to make a configuration file.

And remember, the ultimate goal

of this file is to create our container

inside of our local Kubernetes cluster.

So inside my code editor

I'm gonna make a new file called client-pod.yml.

And then inside of here, we're going to list

out a little bit of configuration to create our container.

Now, I'll be completely honest with you,

I wanna be like as transparent as I possibly can,

these first dozen or half dozen videos

on the topic of Kubernetes,

this is the fifth time that I've gone through

and recorded all this stuff.

So I've recorded four times already

and deleted everything cause I just wasn't happy with it.

So in this try or this run through of recording this stuff

we're gonna do things just a little bit differently.

We're gonna write out all the configuration inside this file

and we're going to write out all the configuration

inside that second configuration file as well.

And then once you see all this configuration

in front of you

we're then going to do a real big discussion and talk

about exactly what every line of this configuration means.

Now, like I said, kind of gone and recorded this stuff

like four times and I'm convinced that

writing stuff out is probably the best way

to you expose to this.

So, the good news is

we're gonna learn exactly what's going on

inside these two configuration files.

The bad news is

that you and I are gonna do

a pretty good amount of typing right now.

So as usual, I gotta ask you

please, triple check your spacing.

Triple check your spelling.

Make sure you got everything dead accurate, the same as I.

Otherwise you're gonna run into a a little bit of issues

down the road.

Okay, so let's get to it.

Inside of here, I'm gonna first type out

API version:v1

I'll do kind pod,

I'll do metadata,

and then I'll do an indent

and say name is client-pod.

I'll say labels,

and then I'll indent another line

and do component:web.

Then now I'm going to go all the way back out.

So, no more indentation.

I'll say spec containers.

And then I'll do a - to indicate an entry inside

env and array.

I'll say name client

image of your docker ID.

So Stephen Grider for me, \ multi-client, like so.

Now remember when we put in this image name right here

that is assuming that our image is up on docker hub.

If your image, for whatever reason,

for multi-client is not on docker hub

you can always make use of my image name, which is

or excuse me, my docker ID, which is Stephen Grider.

I'll then list off my ports that I want to open up

for connecting to from the outside world, just

as we did before over inside of our docker composed file.

I'm gonna have a single entry here

and I'll say, container port is 3000, like so.

Okay, so that's our first configuration file.

Now, again, I gotta ask you,

please, just, I'm begging you,

triple check your indentation.

Triple check your spelling on what you have.

And once you're happy,

we're going to create one other configuration file.

That's gonna do a little bit of networking setup for us

and make sure that the container that we're creating

with the first configuration file

will be exposed or available to the outside world.

So inside my code editor, I'm gonna make another file.

I'm gonna call this one client-node-port.yml.

Okay. And then inside of here, we're just gonna do

a little bit more typing and then we'll talk

about what all this stuff is doing in

more detail than you could ever possibly imagine.

It will be great.

Okay, so inside the new file, we'll say API version is v1.

I'll say kind is service.

We'll do metadata.

I'll give a name of client-node-port.

Then I'm going to un-indent.

So I'm all the way back out to the left hand side

and I'll say spec with a type of node port.

We'll do a port's array.

So I'll do an entry in the array

indicated by this little - right here.

And I'll say Port is 30, 50.

I'll do a target port of 3000

and I'll say node port of 31, 515, like so.

And then finally I'm going to un-indent.

So I'm back online with type and ports.

And then I'll say selector is component:web.

And I'll save the file.

Now again, I gotta ask you, please, please, please,

triple check your spelling inside of here,

triple check the indentation.

Remember inside of a .yml file

if your indentation is not perfect

the file will be misinterpreted

and nothing is gonna work the way you expect.

Okay, so that's all the typing we gotta do.

Now we're gonna take a quick pause right here.

We're gonna come back the next section and we're

gonna go over both these files in just

tremendous detail.