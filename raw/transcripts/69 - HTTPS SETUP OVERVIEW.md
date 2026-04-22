![[Pasted image 20260423001046.png]]
![[Pasted image 20260423001125.png]]
-: All right, my friends, if you are still here,

it means that you want to figure out

how to set up HTTPS support on your Kubernetes cluster.

Now, the first thing I want to mention to you,

and I wanna make sure this is really clear,

is that in order to set this stuff up,

you are going to have to purchase a domain name,

which is about 10 US dollars,

although sometimes you can find ones

for a little bit cheaper than that.

So this is a required step.

You have to have a domain name,

either one already purchased,

or you have to buy one to use with your cluster.

Now the second thing I wanna mention is that you and I

are gonna go through the process of setting up https,

but we're not gonna talk very much

about TLS, or HTTPS, or certificates, or anything like that.

There's a ton of resources out there online already

for you to understand exactly what HTTPS is doing for us,

and the benefit of applying it to any given web application.

So with that in mind, let's talk about the overall process

of how all this stuff is going to work.

All right.

So, we're gonna set up a little bit

of interaction between our Kubernetes cluster

and a certificate authority called Let's Encrypt.

I suspect you've probably heard of Let's Encrypt.

If you haven't before,

essentially it's a service that allows us

to get free certificates.

All we have to do is set up a little bit of communication

between our Kubernetes cluster and a Let's Encrypt service.

So here's the sequence of actions

that are going to be going on behind the scenes.

You and I are not going to actually see these steps occur.

We're gonna kind of peripherally see them occur,

but essentially what we're going to be doing

for setup is creating some infrastructure

to allow these steps to happen.

But again, you and I aren't going to very directly

see these things occur.

So the first thing that's gonna happen

is our Kubernetes cluster is going to issue a request

to Lets Encrypt, and it's gonna say,

hey, I own a domain called multi K eights.com,

and I need a certificate from you, Let's Encrypt,

that says that I do in fact own that domain.

And once we get that certificate, we can then use it

to set up HTTPS that is going to be widely supported

or recognized by any given browser such as Chrome,

Internet Explorer, whatever it might be.

In response, Let's Encrypt is going to immediately reply

and say you own multi K eights.com?

I don't believe that.

I don't believe it for a second.

But I'll tell you what,

I'll give you the benefit of the doubt.

If you, Kubernetes cluster, really do own the domain

multi k eights.com, I'm going to make a request

to multi k eights.com slash well known slash

random string of numbers right here.

If you really own that domain, then you will reply

with some information that I expect to hear from you.

So essentially what's going on right here

is Let's Encrypt wants to make sure

that you and I really truly own this domain.

So it's going to tell us that it's going to make a request

to some route like this right here.

And if we truly own this domain,

you and I are gonna set up a route handler

that's going to reply

on specifically this route right here with some information

that is provided to us by Let's Encrypt.

So that is something that is very challenging

for us to fake.

If we said that we own something like, you know google.com,

and Let's Encrypt replied in the same fashion and said,

okay, well if you really own google.com,

I'm gonna make a request to google.com

slash this random route right here.

Chances are you and I are not going to be able

to manipulate Google's infrastructure

to get the appropriate response to that route.

So that's how this authentication process

is essentially going to go down.

Let's Encrypt is then going to make a request to that route.

And then as long as our Kubernetes cluster responds

with the appropriate data, Let's Encrypt is gonna say,

oh, okay, you know what?

I guess you do own this domain.

So I'll tell you what.

You check out, you own the domain,

so I'm going to give you a certificate

that's going to be good for 90 days.

And after that period, or when that time is about to elapse,

you're gonna have to go through the same process

all over again.

So in some number of days, you're going to have to come back

to me and ask for another certificate

and claim that you own the domain multi k eights.com.

So as long as we set up some infrastructure to handle this,

we'll get our certificate, and we'll be good to go.

Now, to give you a little bit more behind the scenes here

of what you and I actually have to do.

You and I are not gonna set up any route handlers

to facilitate this process.

You and I are not going to be making any requests

over to Let's Encrypt.

We are not gonna do any of that stuff whatsoever.

Instead, we're going to set up another little plugin

through the use of Helm.

Remember we installed Helm just a little bit ago.

We're going to use Helm to install something

into our cluster that's going to automatically

facilitate this process for us.

So we're going to install something into our cluster

that's going to automatically reach out to Let's Encrypt.

That's going to say that we own some particular domain,

that's going to automatically set up a route handler

to respond on this route.

It's going to then automatically get a certificate

back from Let's Encrypt.

It's gonna save it into a secret and make it available

to the rest of our application.

And then after some amount of time elapses,

and the add-on decides that it's time

to renew their certificate, it's going to automatically go

through this process again.

So the challenge for you and me is not setting up,

like this stuff right here.

It's setting up the infrastructure to do this stuff

for us automatically.

That's the challenge.

That's what we need to do.

So with that in mind, let's take a quick pause right now.

We're gonna come back the next section,

and we're gonna start going through this setup,