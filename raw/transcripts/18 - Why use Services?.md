
```bash
aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl get pods -o wide

NAME                                 READY   STATUS    RESTARTS   AGE     IP          NODE             NOMINATED NODE   READINESS GATES

client-deployment-576bc76988-mp7wn   1/1     Running   0          6m50s   10.1.0.15   docker-desktop   <none>           <none>

aryankeshri@Aryans-MacBook-Air-2 simplek8s %
```

![[Screenshot 2026-04-19 at 3.58.34 PM.png]]
![[Screenshot 2026-04-19 at 3.59.00 PM.png]]

-: In the last section,

we finally applied our first deployment

to our local cluster.

Now just to make sure

that everything is working the way we expect,

I want to try to connect to the container

inside of that pod, running the multi-client image.

And when we connect to it

we should see that little react application here

on the screen.

So I want you very quickly to remember

how we connect to our local cluster.

We do not use local host,

we have to run that mini cube IP command.

Remember that mini cube virtual machine

that is our local node has its own IP,

completely different from local host.

So we need to first run mini cube IP.

Once we get the IP address, we'll then make sure

that we connect to the port that was associated

with that service that we created,

which was hard coded to be 31515.

So back at my terminal, I'll do mini cube IP,

there's the IP of my virtual machine.

I'll copy it.

I'll make a new tab inside my browser

and then I'll put on colon 31,515

because again that is the hard coded port of our service.

And when I do so, I do in fact

successfully see my application appear on the screen.

Awesome.

Now this is actually going to be a really good time

to start to understand why we use these service things.

Like why do we have to create that service object?

Why were we not able before to just directly connect

to one of our pods that was running the container

that we care about?

Well, now that we understand the idea behind deployments,

understanding why we have to have these service things

is going to make a lot more sense.

So one thing I wanna show you very quickly,

back over at your terminal,

try running the the command cube ctl,

get pods, and then we're gonna add on an argument here

of dash O wide, like so.

That's going to still get our list of pods

but it's going to append on

a little bit of additional information.

Now I'm gonna zoom out here really quickly

just so I can see this thing on a single line.

There we go.

Now I want you to notice how we now have these IP

and note columns over on the right hand side.

Now the really important column

to take notice of here is the IP column.

Every single pod that we create

gets its own IP address assigned to it.

So when we just deployed our deployment just two seconds ago

and that created a pod, it was randomly assigned

an IP address of 172- 1706.

This is an IP address that is internal

to our virtual machine.

So you and I cannot visit that IP,

or at least we cannot visit it very easily.

It is an IP address that has been assigned

to the pod inside of our node right here.

Now the interesting thing about that is that

if this pod ever gets deleted

or if an additional one is created,

or if for any reason the pod gets updated or changed

in any way, shape or form whatsoever,

it's entirely possible that the pod

might suddenly get a brand new IP address.

So let's imagine that this pod right here

has that IP address of something like, I dunno, the 172.001.

We'll just kind of make up an IP address like so.

So I want you to imagine if we figured out some way

to connect our browser to a pod

running directly inside of our node,

then inside of our browser,

we would type in something like 172001,

and then we would probably want to connect to port 3000.

So we'd probably put on colon 3000 or something like that.

Now the problem starts to arise

if we start to recreate this pod right here

because maybe we have to remake it

because our configuration has changed on the deployment

or maybe the original pod crashed

or whatever reason it might be.

And so we can imagine

that maybe we have to create a second pod,

and this one would get a randomly generated IP

of something like 002,

and then maybe this original pod gets deleted.

Well now we originally had that IP address of 172001.

That IP address is not going to work anymore

because the object or the pod associated with it,

just plain no longer exists.

And so in order to access this pod right here,

we would in theory now have to manually type in,

that 002 like so.

Now as you might guess,

having to update the IP address all the time

inside your browser in a development environment

would definitely be a big pain.

And that's why we make use of these service objects.

This service is going to watch

for every pod that matches its selector.

'Cause remember, that service has a selector.

If we look at client node port.eml

it has the selector of component web.

So the service is going to look for

every pod with that selector

and then automatically route traffic directly over to it.

So that's why we make use of the service.

These pods are coming and going all the time.

They're getting deleted, they're getting recreated,

and every time they potentially might get assigned

a brand new IP address.

This service is going to kind of

abstract out that difficulty.

And that's why we make use of these services

anytime that we want to connect to

one of our different pods.

Okay, so hopefully that gives you a better idea

of why we care about these service things.

So let's take a quick pause right here,

and we'll continue in the next section.