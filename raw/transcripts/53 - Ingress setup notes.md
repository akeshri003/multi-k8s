![[Screenshot 2026-04-20 at 12.37.14 PM.png]]

-: In the last section, we spoke about

how there are two Ingress projects out there

that have very very similar names

and are very similar nature,

but again they are completely separate things.

In this section, I wanna do a very quick follow up

on Ingress NGINX, which is what we are using.

So very important note here;

I want you to understand one other very important thing.

The setup of this Ingress is going to be slightly different

,or in some cases very different,

depending upon the environment that you are using it in.

So if you are deploying this Ingress NGINX thing

in your local environment, or Google Cloud, or AWS,

or Azure, it's gonna be very different

in each of these different environments.

In this course, you and I are gonna set up Ingress NGINX

on our local machine,

and we're also gonna be setting it up on Google Cloud.

So that's kinda like a big reveal here.

Yes, we're going to deploy our Kubernetes cluster

to Google Cloud and not AWS,

even though we used AWS previously in this course.

So that might be a little bit of a surprise.

You might be a little bit disappointed

to hear that we're not using AWS,

but there are some extra-ordinarily good reasons

that we are using Google Cloud over AWS.

And I'll tell you in great detail what those reasons are

when we start moving over to production.

So this is the other note that I wanna throw out there

just to make sure you are 100% aware

that this Ingress stuff, as you probably are able to tell,

it's a little bit finicky right now

in the world of Kubernetes.

You know, you got these identical projects,

the setup is very different depending upon the environment.