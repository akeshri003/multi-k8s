-: In the last video,

we started working on our Scaffold config file.

So we've added in an artifact here.

Remember, that is telling Scaffold

that we want it to manage our client project.

All right, so now the next thing we're going to do,

is go down to the very base layer of indentation here,

so notice how I'm all the way back out.

We're gonna set up a section called Deploy.

We're gonna pass in a couple nested properties here,

so we're gonna say deploy kubectl manifests,

and then we're gonna list out all the different config files

that we want Scaffold to manage for us.

When I say config files,

I'm talking about Kubernetes YAML files.

So essentially, in this case,

we're going to attempt to tell Scaffold

that we want it to manage our multi-client deployment

using the client-deployment YAML file.

So as a array entry right here,

I'll say K8s/client-deployment.yaml.

All right, and that's pretty much it.

So now, anytime we start up Scaffold,

it's going to attempt to apply that config file

to our Kubernetes cluster.

Scaffold simultaneously is going to start watching

our client project directory for changes.

If we make any changes to a JavaScript file, CSS or HTML,

it's going to inject those changed files

into our multi-client pod or container.

One quick thing I wanna mention is that

if we make any changes to a file besides a JavaScript, CSS

or HTML file, then Scaffold is gonna fall back to mode one.

So if we change anything besides CSS, HTML, or JavaScript,

it's going to fall back to just rebuilding that client image

from scratch and using it to update our Kubernetes cluster.

Okay, so that's it for our config,

or just the client deployment.

So I'm now gonna save this file,

then we will do a quick test.

So I'm gonna flip back over to my terminal.

Inside of my complex project directory,

I'm gonna start up Scaffold

by running Scaffold Dev, like so.

All right, there we go.

So you're gonna see that Scaffold's going to immediately

try to build our client image.

It's then going to take that built image and throw it

into our Kubernetes cluster, so you can see

that it created that client deployment right here.

We'll then start to see some logs coming out

of that client image that Scaffold is now,

excuse me, client container that Scaffold is now managing,

let me zoom out here just so we can see those logs

more easily.

And so we can see the message that the React application

has started up successfully in development mode.

We're seeing all this output three times because remember,

our deployment file specifies that we should run

three instances of the client pod.

So now we can flip back over to our browser

and test this out.

I'm going to go to K8smulti.com, remember that is

the host alias I set up on my local machine,

and I'll see the app appear.

And we already really had our application appearing,

so what's different here?

Well remember, anytime we now change a project file,

we should automatically see our React application

get the update.

So to test that out, I'll go back over to my code editor.

In my client directory, I'll find the SRC folder

and then inside there I'll find the app.js file.

Remember, inside of here is that H1 tag

that has that big header at the top,

so I'll just make some change to that text,

I'll say like, "Updated."

So Fib calculator updated.

So I can now save this file.

As soon as I do, I'm gonna change back over to my terminal

and we're going to see how Scaffold responds to that.

So right here it says, "Syncing one file

for Steven Greider multi-client."

So Scaffold has seen that I changed the JavaScript file,

it's gonna take that file and try to inject it

into that multi-client image container.

And we can then see some update messages right here

from those three separate pods

that are running multi-client telling us,

"Hey, all right, we got that update successfully."

The Compiled Successfully message is coming

from Create React app,

so this is essentially Create React app saying,

"Hey, we see a change to a file,

we're going to try to rebuild our client project."

When I say client project,

I'm talking about, like, our client JavaScript code,

not rebuilding an image or anything like that.

So if I now flip back over to my browser,

I'll see Updated appear.

If you do not see the live update right here,

it's totally fine.

I have noticed that Scaffold with injecting stuff

is sometimes a little bit finicky.

You will just about always see the files injected

into the container.

Maybe it's not Scaffold that's finicky,

but rather Create React app is.

So sometimes Create React app

will not automatically update the page.

If it doesn't, just do a manual refresh

and you should then see the update appear.

Okay, so clearly we made a change

and we did not have to rebuild an image manually

or anything like that, we just saw the change appear.

So as you might guess, this is an amazing tool

for local development with Kubernetes,

and there's still a couple more things

I wanna tell you about Scaffold,

so let's take a quick pause right here

and continue in the next video.

  
![Lecture thumbnail](https://mp4-cdn77.udemycdn.com/2019-06-27_19-17-33-cc9cf9d3c44369bbacf969640b4e656b/2/thumb-sprites.jpg?secure=d6w_TvFKWxlPE18wHxChPg%3D%3D%2C1776903660)

4:59 / 5:07

-: In this video, there's a couple odds and ends

I want to mention around Skaffold.

So you'll notice that down here

at the very bottom of the config file,

we added in this manifest section

and we provided a path

to just that client deployment yaml file.

So by adding in this yaml file right here,

anytime we start up Skaffold

with Skaffold Dev at our terminal,

Skaffold will automatically

try to apply that yaml config file

to our Kubernetes cluster.

Now here's the important part.

Anytime we close down Skaffold,

Skaffold will immediately try to delete the deployment

that we created with that file as well.

So take a look at this.

If I flip back over to my browser or see my terminal,

and I hit control+C,

we're going to automatically

see deployment apps client deployment deleted right here.

If I now attempt to get my pods,

I'll see that all of my client pods

have been deleted automatically.

So this is another feature of Skaffold

that's really nice.

Right now, anytime you do some local Kubernetes development,

if you decide to pause working on one project

and start working on another,

you might have a ton of different containers or pods

in the same name space

and it's kind of hard to tell which is which.

So by using Skaffold,

you can automatically set

startup one set of different pods

and then automatically close them all down

as soon as you stop Skaffold as well.

At present,

we are not having our server deployment being managed

or our worker deployment being managed by Skaffold.

So if you wanted to make sure

that those pods also started up

and got shut down automatically,

we could add them to that manifest section.

Let's do that right now.

So back over here, I can add in K8s

and we're going to go for our server deployment

and we can also do our K8s worker deployment as well.

You're not limited just

to having deployments being managed by Skaffold.

We can also add in services as well.

So if we wanted to make sure that we also, say,

shut down the server cluster IP service,

we can just add that as an additional entry.

So I can do server cluster IP service .yaml, like so.

And the same thing for,

let's just go ahead and be complete here,

I'll add in also my client cluster IP service

and our worker doesn't have one

so we'll just do just server and client.

All right, so I can now save this file,

flip back over to my terminal.

I can start everything up with a Skaffold Dev.

So we'll see that Skaffold

has automatically applied all those different config files.

Our React application's gonna start up,

and then at any point in time,

we can hit control+C

and we'll see that Skaffold's gonna shut down

all of our client pods,

all the server pods, and the worker pod as well.

So I'll do that right now.

I'll hit control+C,

that's going to delete all those pods.

And now if I get my pods, yep,

I will see that the server deployment,

while it's still listed,

it is being terminated along with the worker as well.

So if I run that again right now, yeah, there we go.

Now the server and the workers are completely gone.

Now there is, kind of,

one downside to this entire approach of adding in

all these different services or manifests

into this Skaffold file right here.

'Cause remember we're now saying

that as soon as we close down Skaffold,

we're going to essentially delete

all the things we have listed here.

So the downside to that

is that if you have anything persistent,

like maybe a database or a volume

that you want to keep around for development purposes

because maybe you've got a lot of test data loaded on there

or something like that,

do not add those things into your Skaffold file

because if you have any persistent data

in the form of volumes or pods or whatever else,

Skaffold is gonna delete that stuff

as soon as you close it down.

So I would recommend

not adding those things to your Skaffold file.

And you can actually see in my environment right here,

I actually have a Postgres deployment

that's been running for 110 days.

So that's for a very different project

that I've been working on for quite a while.

I don't want that deployment for Postgres

to be managed by Skaffold

'cause I have some persistent test data in there

and I don't want to have to be recreating that database

and reseeding that database with all that test data

every single time I decide to go back

and start using Skaffold on it again.

So in the case of all your different pods

for running React applications or APIs

or, kind of, these stateless services,

no problem adding them to the Skaffold file.

But if for anything related to data,

you might not want to add it in

because, like I said,

Skaffold is just going to very quickly delete it.

All right,

so now that we understand a lot more about Skaffold,

I wanna add in all of our different services or deployments

to this thing so that we can get some live updates

on our server, and our worker,

and all that good stuff as well.

So let's take a quick pause.

When we come back,