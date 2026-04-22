
-: In the last section, we spoke about

how we're going to take our multi container project

and update it to use Kubernetes

as opposed to the Docker Compose

or Elastic Beanstalk setup that it currently has.

Now you will notice that in the last lecture

I posted the zip file for the complex project.

I put it there

just in case you made any changes to the project.

That is a backup of my exact files as they stand right now.

So I took my exact complex folder directory

with the server, the client, the worker

the whole nine yards, and I put it into that zip file.

So again, if you made any changes to the project whatsoever

between the last video

that we were working

on it and right now,

I highly encourage you to

download that zip file and use that

as the starting point for all this Kubernetes stuff.

Now in this section we're going to take the complex project

or the multi container project

and we're going to start it up with Docker Compose.

The reason that we're going to do this is to just make sure

that everything is still working the way we expect.

Because if we start building out all this Kubernetes stuff,

but some of the underlying project code got changed

by you at some point in time, or it's in a broken state,

doing the Kubernetes stuff

is going to be basically impossible, completely impossible.

So we just need to make sure that the code that you're using

and the code that I'm using is still working

and a hundred percent up to date.

All right, so you'll notice

that I'm inside of my complex project folder right here.

The other thing that I want you to triple, triple check

make sure that you are not using a copy

of Docker that has been configured to run inside of our

or to access the Docker server inside of your node.

So you can very easily do that by doing a Docker PS

and you should see that you have very few

or absolutely no containers running whatsoever.

If you see a ton of printout,

that means that your Docker client

is still configured to

access the Docker server inside the node.

We do not want to start up Docker Compose

inside the node, whatsoever.

If you ha are still using the updated copy

of Docker or the reconfigured version,

remember, all you have to do to get back

to the non configured version is

open up a new terminal window.

That's pretty much it.

Just open a new terminal window and do a Docker PS again

and you'll be back to using

your local copy of Docker server.

Okay, so again, I'm inside

of my multi container project directory.

I see the Docker Compose .YAML file right here.

I'm going to run Docker Compose up

and rebuild all these images.

And again, we're just doing this to make sure

that all the existing code works as we expect.

So I'm gonna do a Docker Compose up --build,

because I want to explicitly rebuild everything.

And I'm gonna let that run.

It's gonna take a couple minutes to rebuild everything

and launch it all.

Now you'll notice in my case,

I kind of just very recently

rebuilt all this stuff,

so it launches very quickly.

I do want to remind you

that the very first time you launch all these containers,

if you cleared out all the cached version

of the containers and whatnot,

it might crash a little bit

the very first time you launch it.

So I do encourage you to

stop Docker Compose immediately after it starts

up and do a Docker Compose up a second time.

And the second time

you don't need to add on the --build flag.

All right, So I'm gonna launch that.

I'm gonna wait just a second

for the development server to start up.

And then you'll recall

that we can access all this stuff on local host port 3050.

I already opened up my code editor

inside that complex directory.

Here's the Docker Composed file.

Here's the Nginx server

that we are using for routing inside of our application.

And you'll recall that we had set up local host 3050

as being the central point to access everything

inside of here.

So I'm just gonna very quickly test this out

by opening up my browser and going to local host 3050.

And again, I should still see all of my numbers here

Any previous values that I had entered

unless you had of course deleted or completely stopped

and deleted the Redis in the Postgres containers.

And just to make sure

that everything's working the way I expect,

I'm gonna enter in a new index here of 12.

I'm gonna submit it,

I'll then refresh and I'll see 12 added here

and I'll see a calculated value for 12 down there as well.

Okay, so hopefully you see something like this.

Again, if you don't see everything updated

and still working like so,

then I really encourage you to go back

to the previous lecture, download that zip file

and use that as the starting point

for all this Kubernetes stuff,

just to make sure that you and I are on

a 100% equal footing.

Okay, So let's take a pause right here.

When we come back in the next section,

we'll get started with taking all of our existing images

and migrating them into the world of Kubernetes.

So quick break and I'll see you in just a minute.![[Screenshot 2026-04-19 at 8.18.14 PM.png]]