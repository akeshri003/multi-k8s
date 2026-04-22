
![[Screenshot 2026-04-19 at 3.21.21 PM.png]]

-: In the last section, we made a very small change

to our pod configuration file,

updating its container port.

But when we did so,

we very quickly saw an error message

that said that we were not allowed

to update certain fields that are tied to a pod.

And so essentially that message is telling us

that there are some fields,

so we are allowed to update such as the image

that a pod uses.

But there are other fields we are not allowed

to touch at all once the pod has been created.

And so this kind of flies in the face

of everything I just told you about making changes

to a configuration file.

In other words, how are we gonna change a configuration file

to change an object inside of our cluster

if there are fields

that we are not at all allowed to update or touch?
![[Screenshot 2026-04-19 at 3.23.11 PM.png]]


![[Screenshot 2026-04-19 at 3.23.26 PM.png]]![[Screenshot 2026-04-19 at 3.26.27 PM.png]]

Well, to solve this issue,

we're gonna start making use of a new type of object

that is going to replace our usage of pod inside

of our very simple application as it stands right now.

So the new type of object that we're going to make use of,

in addition to pods and services

is something called a deployment.

A deployment is a Kubernetes object

that is meant to maintain a set of identical pods.

So that can be one pod, two pod, three pod,

whatever number, and the deployment

is going to constantly work

to make sure that every single pod

in its set that it's supposed to manage

is always running the correct configuration,

and is always in a runnable state.

In order words, it is not crashing

or it's not dead or anything like that.

Now, a deployment is very similar in nature to a pod.

Yes, I just said a deployment contains a set

or maintains a set of pods

but at the end of the day,

we can use either deployments or pods

with Kubernetes to run containers for our application.

So let's compare and contrast some of the differences

between pods and deployments.

So with a pod, we're running a single set

of very closely related containers.

Remember, in a pod,

we're only gonna stick in multiple containers

if they have very tight integration with each other.

In reality, pods are only used

in a development environment.

And usually only if you have

like a very one-off single container

or a very small group of containers that you want to run.

We do not actually make use of pods directly

in a production environment

because of these limitations around being able

to update its configuration and stuff like that.

Now, a deployment, on the other hand,

again is meant to set

or is meant to run and manage a set of identical pods.

So that is one or more.

That can be one pod, two pod, three pod,

however many you want.

In that set of pods,

every pod is going to be running the exact same set

of containers with identical configuration.

Now, the deployment itself

is going to monitor the state of each pod.

It's gonna watch the configuration of each one.

It's gonna make sure that every pod

is running the container successfully inside of it.

If any pod happens to crash for any reason,

the deployment is going to automatically attempt

to restart that pod or completely recreate it

in a fresh new state.

We make use of deployments

in a development environment,

and we use them as the primary means

of running containers in a production environment as well.

So in this course, I first started off showing you pods,

just so you understand here's a very basic way

of creating a container with Kubernetes.

But in reality, when we start making use

of Kubernetes for any serious purpose,

we make use of deployments

as opposed to individual pods.

So from here on forward,

we're going to kind of tend to forgot that pods exist,

and we're going to instead make use of deployments

for running all of our different containers.

Again, behind the scenes, a deployment is just making use

of pods, so it's still very important

to understand what a pod is

and how you work with one,

but again, we're going to make use of deployments

in our development and production environments.

Now, the last thing I wanna show you

is just a very quick diagram here

of what's kind of going on behind the scenes.

So when we create a deployment object,

it's going to have attached to it something

called a pod template.

A pod template is essentially a little block

of configuration file,

or excuse me, a little block of configuration

that says hey, here's what any pod

that is created by this deployment is supposed to look like.

So the pod template might say okay,

every pod that this deployment manages

is supposed to have one container

that has the name of Client

that exposes port 3000

and uses the image multiworker.

And so that deployment would use this template

to create a pod that looks like this.

A pod that has the name of oh,

not Client Pod, just simply Client.

It's running the multiworker image

and it exposes port 3000 to the outside world.

If we made a change to the pod template over here,

like let's say we're instead supposed to expose port 3000.

We can change that to 999 like so.

Then the deployment would attempt

to either change the existing pod that it's managing

or alternatively, it would attempt

to kill this pod entirely

and replace it with a brand new pod

that has the correct port assigned to it, like so.

So again, this deployment object over here

is going to be constantly watching all of the different pods

that it maintains.

It's gonna be watching their state

and making sure that they have the correct state.

So at the end of the day,

to solve this issue that we're having right how

with updating the configuration for our pod,

rather than trying to make use of a pod

and update the container port,

we're going to instead refactor this thing

to instead be a deployment

that creates a pod running the multiworker image

and with a very specific container port.

Once we make use of the deployment,

all those restrictions around updating certain variables,

like say the container port right here

or the name of the container will be lifted.

With a deployment, we can change any piece

of configuration tied to a pod that we want to.

We don't have to worry about seeing that error message,

like we did previously.