![[Screenshot 2026-04-17 at 10.48.55 PM.png]]

`kubectl apply -f client-pod.yaml`

```
aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl apply -f client-pod.yaml

pod/client-pod created

aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl apply -f client-node-port.yaml

service/client-node-port created
```
![[Screenshot 2026-04-17 at 10.53.07 PM.png]]
![[Screenshot 2026-04-17 at 10.55.35 PM.png]]

3050 -> port that other pods would use.
31515 -> pport that developers would use to access. 
targetPort is not reflected why?

Behind the scenes - 
![[Screenshot 2026-04-17 at 10.58.19 PM.png]]

the ports of vm are not available using localhost if you are using minikube,  to access them, you need to have the ip address of the virtual machine you were using. 
But if you are using docker desktop to run kubernetes, which our case we are, then we can access it using localhost.
to get that ip address, you can use - commands like if you are using minikube - 
```
minikube ip
```

Raw Transcripts - 
Instructor: In the last section, we finished

going through both of our configuration files,

and we're now ready to load these

into our Kubernetes cluster

and try to access our running container.

So in order to do so,

we're going to feed both these configuration files

into our cluster through the kubectl command line tool.

The command in particular that we're going to run

is the apply command.

So at the command line,

we're going to run kubectl apply -f,

and then the name of a configuration file,

or I should say the path to the configuration file.

Now, kubectl, again, that's a command line tool

that we use to mess around with everything going on

inside of our Kubernetes cluster.

The apply keyword means that we want to

change the configuration of our cluster.

And that's actually kind of a very loaded term,

like these words right here

change the current configuration.

That's a real loaded term right there.

And it's something that we're going to expand upon

at great length in just a little bit.

We then specify that we want to load in a file

that contains all this configuration that we care about.

So we're going to add on the -f flag,

and then the path to the file is the path to the file.

That's pretty much it.

So let's try flipping over to our command line.

And we're going to run this command two times,

one time for the client-pod.yaml file

and one time for the client-node-port.yaml file.

So back in my terminal, I'm going to first make sure

that I'm inside of my simplek8s directory,

which is where I have those two configuration files.

I'll then run kubectl apply -f.

And let's first load up the client pod.

So I'll say, "client-pod.yaml."

I'll hit enter,

and then we'll very quickly see

that the pod has been configured.

Now, what does configured mean?

Who knows, that seems a little bit mysterious.

It doesn't really seem to say that the pod was like,

or the container was successfully created.

And, in fact, this message printed up

a little bit too quickly for me to

really feel like the container was successfully created.

So we might need to do a little bit

of a status check here in just a second

to make sure that the pod

was in fact started up successfully.

But before we do, let's apply our other configuration file.

So I'll do kubectl apply -f.

And then the other file was called client-node-port.

So client-node-port.yaml.

And, again, I see configured message right here.

Now, again, in both cases,

seems like these messages appeared pretty quickly,

so I don't really know if everything worked

the way we would expect.

So let's try figuring out how we can print out the status

of both these different objects that we just created

and just make sure that

they were, in fact, successfully created.

So in order to get the status

of any different object that you and I create

through a configuration file or any other means,

we're going to be making use of the kubectl get command.

The get command is going to print out the status

of an entire group of object types.

So, for example, we would say, "kubectl get pods."

In this case, pods is the type of object

that we want to get information about.

Kubectl will look at our Kubernetes cluster,

it'll find all the different pods that have been created,

and it'll print out the status of every single one.

So it's a very easy way to get a

kinda high-level look at everything that's going on

inside of your cluster.

So I'll do kubectl get pods at my terminal.

And I'll see that I have one pod created.

It has a name of client-pod.

One of one right here

means that there is one copy of it running.

And we need to have one copy running.

So the first number right here

is the number of pods that are running

and the second number is the number of copies

that we want to have.

As we start to scale our application,

we might want to have multiple copies

of the exact same pod running.

And so when that happens,

you would expect to see one right here

change to two, three, four, five, whatever it might be.

We then see that the status is running.

So it definitely looks like everything started up A-OK.

There have not been any restarts.

So if your pod right here crashes for any given reason,

it'll be automatically restarted

and you would see restarts right there increment by one.

And then age of one hour means

it's been running for one hour.

Now in my case, the thing has not actually

been running for one hour.

I just started up this pod a while ago

and I forgot to close it before running the apply command.

But not a big deal.

You're gonna see a age of like

one minute or something like that.

Okay, so now we're going to get the status

of all the different services that we have created as well.

So to get a printout of all of our different services,

we're just going to slightly change the kubectl get command.

We're gonna say that we want to get a printout

of all the objects with type service that we have created.

So I'll say, "kubectl get services."

And that's gonna print out two different services.

Now, chances are you have two services running as well.

One might be Kubernetes with a type of ClusterIP.

If you see that,

that's one of the inner workings of Kubernetes

and you can completely ignore that thing.

Now, hopefully, you're gonna see

a second service printed out in here

with the name of client-node-port.

Its type will be, of course, NodePort.

It'll have a cluster-IP, an external-IP,

and then a listing of ports and age over here as well.

Now, I want you to look very closely at ports over here.

You'll notice that there's both the port property

and the node port property as well.

And so as a quick reminder, the first number on there,

where's our diagram?

Here we go, so the first number on there is the port.

That is the port that other pods or other objects would use

to access the pod that this service points to.

Second number is the node port.

And so that is the port that you and I would use

to access that service inside of our browser.

You will notice that the one port

that is not reflected inside of here is the target port.

So the service does not report

the port that is trying to open up inside the target pod.

That's not done for any security issues.

It's just done because, like, who cares? (chuckles)

Who cares, right?

For that print up,

you probably don't care about the target port.

You probably only care about the port property

and the node port.

All right.

So that's pretty much it.

We have now deployed both of these objects

and they're now running on our local cluster.

So now the very last thing we have to do

is attempt to access our running multi-client project

inside of our browser.

Now, you might expect,

and based on my wording right now, you'll,

as you might guess, this is not the way we do it,

you might expect that we would go to localhost:,

and then whatever we put in as the node port over here,

31515.

Remember, this is the port that we use to access

or test out our container inside the browser.

So you might think that we go to

localhost:35151 or whatever.

I keep mistyping that thing.

I'm just gonna do a copy paste.

There we go.

So, of course, when you go there, yeah, nothing works.

So what's going on?

Well, remember what is going on behind the scenes

on your computer right now.

Let's pull up a good diagram for this.

This one right here, this'll do.

Nah, let's do this one.

Okay.

So when we are inside of our browser,

when we want to access some container

that is running on that Kubernetes node VM

created by Minikube,

this is not addressed by localhost.

In other words, all the ports

that exist inside this node VM right here

are not available on localhost.

In order to access this VM, we need to actually ask Minikube

for the IP address that was assigned

to this virtual machine

when it was created on your computer.

So this virtual machine right here

that was created on your machine has its own IP address,

and you need to visit that IP address

in order to access any of the different services,

any different pods that are running inside of here.

So to access that IP address,

we'll flip back over to our command line,

and we're going to run minicube ip.

And that's gonna print out the IP address

of that virtual machine.

So at any point in time,

forever inside this course and your own applications,

anytime that you want to access

some application that is running inside of Minicube

or inside that virtual machine,

you are not going to use localhost.

Just forget it, there is no localhost, period.

No localhost.

Anytime that you want to access a service,

or a container, or a pod, or whatever it might be,

that is running on your virtual machine

or inside of your Kubernetes cluster,

when you're running in development mode,

you're going to run minicube ip

and you're going to use this IP address right here.

So, hopefully, I made that memorable enough. (laughs)

You're not gonna use localhost, that's pretty much it.

So we're gonna copy this right here.

I'm gonna go back over to my browser,

I'm gonna put in that IP,

and then we're going to specifically access the port,

the node port that we set up on that thing.

So I'll say, ":31515."

And I'm going to put that over inside my command line

just to make sure it's really clear.

Yeah, there's a colon on there.

Remember, your IP address might be very different than mine.

So do not go to my IP address.

Go to whatever IP you see printed out at your terminal.

All right, so we're going to go there.

And lo and behold,

we see our application appear on the page.

Now, of course, if you open up your terminal,

you're gonna see a couple error messages

because this thing is not able to access the Express API.

That's totally fine.

It's just because we have not actually set up that API

inside of our Kubernetes cluster yet.

Okay, so that's pretty much it.

We have gone through the entire process

of creating two different objects,

both a pod and a service.

We understand the differences between the two

and we understand that we have to create a service

if we want to access anything inside of our running pod.

Remember, by default, Kubernetes is far more restricted,

with all of its networking stuff,

than anything we ever did with Docker Compose

or with Elastic Beanstalk.

With Kubernetes, we have to be very, very explicit

about all the networking that we want to set up.

And we do all the networking setup

by the creation of these different service things.

So that's pretty much it.

That's the basics of Kubernetes.

So let's take a quick pause right here,

and we're gonna start looking at some more complex stuff

in the next section.