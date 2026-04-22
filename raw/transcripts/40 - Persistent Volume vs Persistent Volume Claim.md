![[Screenshot 2026-04-19 at 11.11.45 PM.png]]
![[Screenshot 2026-04-19 at 11.12.46 PM.png]]
-: In the last section,

we spoke about what a volume is,

compared to a persistent volume.

We're now going to pivot a little bit

and talk about how a persistent volume claim

compares to a persistent volume.

Now, these two topics right here,

and their relation with each other

is kind of hard to understand at first glance.

So I'm gonna give you a very quick analogy

or a very quick story

to help you understand what the difference

is between a volume claim and a persistent volume.

After I give you the analogy,

we'll then kind of take this little short story

and pivot it over to the world of Kubernetes

and apply some real terminology to it.

So in this section, let's get to it.

We're gonna look at a very quick little short story

to help you understand the difference

between a persistent volume and a persistent volume claim.

So here's my short story.

I want you to imagine that you are walking down a sidewalk,

and you are in the process

of building out some custom computer.

You are building a computer from scratch,

and so maybe at this point in time you have a case,

and a motherboard, and a processor, some RAM

and a Graphics Cards.

And the last thing that you need

might be some type of hard drive.

So you're walking along with your custom PC

and you see a big billboard that says,

"Come on down to the computer store,

we have two really great hard drives that are available.

We have a 500 GB option and a 1 TB option."

So you look at this billboard right here and you say,

"You know what, between these two options,

I think that 500 GB hard drive

would be perfect for my custom computer."

So you take your custom computer,

you continue walking down the sidewalk

until you get to the computer store.

Now at the computer store,

you talk to a salesperson and you say,

"Hey, you know I just saw that billboard

where you were advertising the 500 GB option.

I would love to have one of those hard drives."

So the salesperson says, "Not a problem."

And they turn around and they go to some store room

or some like inventory store, inside the computer store,

and they look through all the different pieces

of hardware that they have available.

Now, you had asked for a 500 GB hard drive,

so the salesperson says, "Great, this is perfect.

I can meet your request right now,

I've got a 500 GB hard drive, ready to go."

And so they throw that thing over to you

and you now have a 500 GB hard drive

that you can put into your computer.

All right, so that's the entire short story.

That's kind of scenario one.

Now I wanna, very quickly,

go through the same scenario again,

but we're going to imagine that rather than asking

for a 500 GB hard drive,

maybe this time around you ask

for a 1 TB hard drive instead.

So we're gonna do a second flow

![[Screenshot 2026-04-19 at 11.15.30 PM.png]]

![[Screenshot 2026-04-19 at 11.17.17 PM.png]]
![[Screenshot 2026-04-19 at 11.18.56 PM.png]]
![[Screenshot 2026-04-19 at 11.19.33 PM.png]]
  
![Lecture thumbnail](https://mp4-cdn77.udemycdn.com/2018-08-24_22-45-47-3adfd289b98b4984eb6412c4591e6e19/2/thumb-sprites.jpg?secure=Sh1KdWSPYp69XH56MqG3QA%3D%3D%2C1776636638)

9:18 / 9:22

-: In the last section,

we spoke about what a volume is,

compared to a persistent volume.

We're now going to pivot a little bit

and talk about how a persistent volume claim

compares to a persistent volume.

Now, these two topics right here,

and their relation with each other

is kind of hard to understand at first glance.

So I'm gonna give you a very quick analogy

or a very quick story

to help you understand what the difference

is between a volume claim and a persistent volume.

After I give you the analogy,

we'll then kind of take this little short story

and pivot it over to the world of Kubernetes

and apply some real terminology to it.

So in this section, let's get to it.

We're gonna look at a very quick little short story

to help you understand the difference

between a persistent volume and a persistent volume claim.

So here's my short story.

I want you to imagine that you are walking down a sidewalk,

and you are in the process

of building out some custom computer.

You are building a computer from scratch,

and so maybe at this point in time you have a case,

and a motherboard, and a processor, some RAM

and a Graphics Cards.

And the last thing that you need

might be some type of hard drive.

So you're walking along with your custom PC

and you see a big billboard that says,

"Come on down to the computer store,

we have two really great hard drives that are available.

We have a 500 GB option and a 1 TB option."

So you look at this billboard right here and you say,

"You know what, between these two options,

I think that 500 GB hard drive

would be perfect for my custom computer."

So you take your custom computer,

you continue walking down the sidewalk

until you get to the computer store.

Now at the computer store,

you talk to a salesperson and you say,

"Hey, you know I just saw that billboard

where you were advertising the 500 GB option.

I would love to have one of those hard drives."

So the salesperson says, "Not a problem."

And they turn around and they go to some store room

or some like inventory store, inside the computer store,

and they look through all the different pieces

of hardware that they have available.

Now, you had asked for a 500 GB hard drive,

so the salesperson says, "Great, this is perfect.

I can meet your request right now,

I've got a 500 GB hard drive, ready to go."

And so they throw that thing over to you

and you now have a 500 GB hard drive

that you can put into your computer.

All right, so that's the entire short story.

That's kind of scenario one.

Now I wanna, very quickly,

go through the same scenario again,

but we're going to imagine that rather than asking

for a 500 GB hard drive,

maybe this time around you ask

for a 1 TB hard drive instead.

So we're gonna do a second flow

through this diagram.

And imagine that you instead decide

you want the 1 TB drive.

So in this case,

you are, again, walking down the sidewalk.

You see the billboard

and you see that there

is a 1 TB hard drive storage option.

You say, "Oh, that's perfect.

I would love to have a 1 TB hard drive."

So you walk down to the computer store, a second time,

and you say to the salesperson,

"Hey, you know I would love to get one

of those 1 TB hard drives."

And the salesperson says,

"Okay, sure.

Let me just check the store room really quick."

So the salesperson goes back to the store room

and they look at all the hard drives

that they already have in stock.

So these hard drives right here,

inside the store room are essentially hard drives

that have already been created.

They are in existence already.

The salesperson looks through all the stock

that they have and they very quickly realize,

oh no, there is no 1 TB hard drive on stock, on hand,

ready to be sold.

But the salesperson is not going to give up very easily.

The salesperson says, "You know what?

I'm gonna make sure you get what you asked for,

because we were advertising that as an option."

So the salesperson picks up the phone

and very quickly makes a call out to the hard drive factory.

They call a factory and they say, "

Hey, look I got someone right here,

they want a 1 TB hard drive.

I need you to create this thing right now

and ship it on over."

And so the factory puts that hard drive together,

lickety-split, just as fast as you blink,

and they ship it on over to the computer store.

And so now the salesperson has your 1 TB hard drive

that was fabricated, on the fly, just for you.

And they hand the hard drive over,

and you are now a happy little camper

because you have the 1 TB hard drive

that you were asking for.

All right, so that's the entire story, two run throughs.

Now, there's a couple of very quick,

important points that I want to highlight here.

The first important point, is that we had a billboard

that was essentially advertising storage options

that were available.

The second important thing,

is that for those two storage options,

some of them were ready to go and preassembled.

So there were some instances of 500 GB hard drives

that had already been created

and were ready to be handed off to you, the customer.

There were also some storage options

that had been advertised on that billboard

that were not ready to go,

and had to be essentially fabricated

or created, on the fly, to meet your demand.

So, whether it was a hard drive option that was ready to go,

or one that had to be fabricated on the fly,

no big difference to you, the customer,

either way you got what you were asking for.

All right.

So that's the two big points I want you to understand.

So we're not gonna look at another copy of this diagram

that has some Kubernetes terminology applied to it.

Okay.

So here's the Kubernetes version of this diagram.

So first off, you and the previous diagram

were putting together a computer,

and you had realized that you needed a hard drive.

That's very similar to you and I, as developers,

putting together a pod configuration.

When you and I put together a pod

that we know is going to need a persistent volume,

we have to look at a billboard of sorts,

that's going to advertise a couple

of different storage options.

So these different storage options

that are being advertised are what we refer

to as persistent volume claims.

So a persistent volume claim is an advertisement.

It is not an actual volume.

It can't store anything, it's just an advertisement

that says here are the different options

that you have access to for storage

inside of this particular cluster.

You and I, as developers, are going

to write out inside of some config files,

the different persistent volume claims

that are going to be available inside of our cluster.

So you and I are going to write a config file that says

there should be a 500 GB hard drive option available

to all the different parts inside of our cluster.

And we might also write out a config file

that says there is a 1 TB option

that is available as well.

So again, a persistent volume claim

is like an advertisement.

It's saying here is something that you can purchase,

here is something you can get

for your pod when it is created.

Now, when you chose one of those persistent volumes,

you went off to Kubernetes with your pod config,

and you said to Kubernetes,

which was the salesperson, in reality,

"Hey, I just saw that 500 GB hard drive option."

And you said to Kubernetes,

"Hey, I want that 500 GB option, give me one of those."

And so Kubernetes had to go back into some imaginary store,

and it had to look through some number of options

of persistent volumes or storage options,

instances of storage options that were readily available.

And so inside of a Kubernetes cluster,

we might have some number of persistent volumes

that have been created ahead of time.

These are actual instances of hard drives, essentially,

that can be used right away for storage.

Any persistent volume that is created ahead of time,

inside of your cluster, is something that we refer

to as statically provisioned.

So a statically provisioned persistent volume,

is something that we have, very specifically,

created ahead of time.

On the other hand,

we also had a another option that could have been created

on the fly.

So this is where referred to you

as a dynamically provisioned, persistent volume.

It is another storage option

that is not created ahead of time,

it's only created when you putting together your pod,

ask for it.

So you can totally ask for this 1 TB hard drive.

You know you can say it to Kubernetes,

"Give me that 1 TB option,

but that 1 TB hard drive was not going

to be created until you went ahead and asked for it."

So that's the difference between a dynamically provisioned

and a statically provisioned, persistent volume.

Is it created ahead of time,

or is it created just when you immediately ask for it?

All right, so that's it.

That's the big difference between a persistent volume

and a persistent volume claim.

The persistent volume claim is a advertisement of options.

You can ask for one of those options

inside of your pod config.

And when you do,

Kubernetes is going to look at its existing stores

of persistent volume,

and it's either going to give you a volume

that's been created ahead of time,

or it's going to attempt to create one on the fly.

So there's the entire example.

Let's take a quick pause right here.

When we come back to the next section,

we're going to start to update our config files

to create a new persistent volume claim

that's going to create a storage option

that can be claimed, essentially, by our Postgres pod

that we had already created.
