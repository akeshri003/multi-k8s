Instructor: In this section

we're gonna start making some changes

to our deployment configuration file

and just verify that all the pods that are associated

with this deployment successfully get updated.

So in particular, I really want to try updating

the container port value down here

from 3000 to some other value,

because you'll recall that

when we tried changing that container port

inside of our client.pod,

or excuse me client-pod.yahml file

we very quickly got an error message saying,

hey, you can't update that value.

Okay, so back inside of the client deployment.

So we're in the deployment file right now.

I'm gonna find the container port

and I'll update the port to 99999 like so.

Now, just so you know,

when we change this port assignment

you will no longer be able to visit

our application inside of the browser.

So in case you apply this update

and then try to test it out inside the browser,

it's not going to work because the container doesn't care

about port 999, it wants you to expose traffic on port 3000.

Okay, so now that we changed our configuration file,

I'm gonna make sure that I save the file

and then I'm going to go back over to my terminal

and we're gonna do the same thing we always do

anytime we want to update an object.

We'll do a kubectlapply-F clientdeployment.yaml.

So I'll run that, and then it tells us very directly

that an existing object type with this name was configured.

It says configured as opposed to something being created.

So we definitely just made a change

to an existing deployment as opposed

to creating a new one entirely.

We can now do a kubectl get deployments

to print out all of our deployments.

Yep, it's still there.

I can do a kubectl get pods,

and now with this you're gonna see something

a little bit more interesting.

Notice how the age of this pod right here is 26 seconds.

So clearly when we apply this configuration file right here

Kubernetes noticed that we made a change to the template

for the pods that are being controlled by this deployment.

It saw that we changed the container port,

and so rather than trying to update the existing pod

that already existed

it deleted that pod and completely recreated it

with the new container port value right here.

Now one thing that would be really nice to do

is make sure that this new pod right here

is in fact running with that new port.

So I'll do a kubectl describe pods.

We'll get the long form description

about this pod right here and we'll probably

be able to verify that port 999 is assigned to it.

Now remember when you do describe pods,

the name of whatever you wanna look up

is completely optional.

In this case, we only have one pod,

so I'll just print out the information about all pods

because hey, there's only one.

Okay, so when I run that,

I'll scroll up a little bit

and then I should be able to see

our list of containers.

Here's the client container

and it has port 999 mapped on it.

So without a doubt this pod is running

with the most up-to-date configuration.

Okay, so that's pretty cool.

Now I wanna do one other change

or one or two other changes here.

Let's first try scaling up our replica setting right here.

So when we scale up from one to something like say five,

we're now going to have our deployment

attempt to create five different pods

all with this identical template right here.

So I'm going to make sure I update replicas to five,

I'll save the file and then as usual

we'll do a kubectl apply client deployment.yaml.

Now if you type in very quickly kubectl get deployments

we might okay a little bit too slow

in that case, you'll notice that

everything says five right here.

If we were a little bit faster

we might be able to see that there are less than five

of these different pods available.

We'll make another change in a second

that we'll hopefully be able to

if we type in this get deployments fast enough

we might be able to see the update in action

and we'll see something like,

hey, only five or maybe one

or even zero pods are currently available.

So if we now do a kubectl get pods

you'll notice we have five separate pods

each of which are running a unique copy

of that very specific client,

let's see, multi client that's the idea

or that's the name of it.

Multi client image.

So we now have five containers running

each in their own very separate little pod.

Let's try making one more configuration change.

So how about instead of running the image multi client

let's try running the image multi worker instead.

So I'm gonna change the name of the image

that we're running inside this pod.

I'm gonna make sure I save the file.

And now this time after we do the

kubectl apply-F client deployment

I'm gonna, very quickly right after I run this command

I'm gonna very quickly do a kubectl get deployments

and hopefully you'll see some different numbers

of containers, excuse me,

different numbers of pods inside there

as the deployment is creating

and destroying some of the pods inside of our cluster.

Okay, so I'm gonna hit Enter here,

and then very quickly I'll do a kubectl get deployments.

Now that's interesting.

So notice I typed in fast enough

that I was able to see

some different numbers in action here.

So the client deployment wants to have

five different pods with the very specific

configuration listed right here.

But at present there are seven pods.

That means that maybe five of the old version exist.

Or actually, you know what,

we can use these other numbers here to figure it out.

So there are seven pods total

and four of them are up-to-date and available.

That means that we have four up-to-date and available pods

running the newest configuration that says

that we should be running the multi worker image.

That means that there are three other pods

because seven minus four is three,

means that there are three pods still sitting around

using the old configuration,

and those need to be deleted and cleaned up.

If we now do kubectl get deployments again

we're probably gonna see five across the board

because now all the old versions

of those pods have been deleted

and replaced with the latest up-to-date version

of the pod spec, which says that we need to have

the multi worker image running inside there.

Okay, so that's pretty interesting

to see these deployments in action.

So let's take another quick pause.

We're gonna come back to the next section

and I wanna point out one or two kind of interesting things

about this entire deployment system

that might trip you up when you start making use of it

in a production environment.

So quick pause and I'll see you in just a minute.