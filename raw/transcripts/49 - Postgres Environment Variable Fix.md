![[Screenshot 2026-04-20 at 11.37.52 AM.png]]
![[Screenshot 2026-04-20 at 11.38.06 AM.png]]
![[Screenshot 2026-04-20 at 11.38.34 AM.png]]
In this section we're going to wire

up our Postgres secret that we just created

in the last section to our server deployment

and our Postgres deployment as well.

So we're gonna first start inside of our

server deployment.yaml file.

I'm gonna scroll down to my list of environment variables

boots, because we want to provide this password

as an environment variable.

So at the very end of of this list,

I'm gonna add on a new name.

The name is going to be pgpassword.

Now something to make clear here, is that

this is the name of the environment variable.

So this is how our secret,

or our encoded password is gonna show up

inside the container.

The name of pgpassword right here is

not at all related to the secret,

and in fact, it could be something totally different.

It could be mypassword,

or whatever we want it to be.

But in our case,

our copy of the multi server image is going to be looking

for a pgpassword environment variable.

And so, that's why we are going to use specifically

pgpassword as the name.

Then, rather than specifying a value property,

we're going to provide a value from property.

So we're essentially saying, get the value

for this environment variable from some configuration

that we're going to put in here.

And so, we're going to put in a secret key ref,

and then we'll provide a name.

The name is going to be the name of the secret

that we want this environment variable value to come from.

So the name of our secret

that we just put together was pgpassword,

all lowercase right there.

So as my name, I'll put in pgpassword.

And then, we also have to put in a key.

Remember that a secret can store many key value pairs.

We only put one key value pair in here,

but we could have very easily added in

several other key value pairs as well.

And so, we need to point out the key value pair

that we want to shove into this environment variable.

So the key that we want to reference is pgpassword.

So for the key, I'll put in pgpassword, like so.

So now that we put the name and the key right here,

Kubernetes is going to automatically open up

this secret with the name of pgpassword.

It's going to find the key value pair

inside of there, equal to key,

so that's this one right here.

It's gonna find the value associated with it,

which is 12345asdf,

and it's gonna pass that into our container

as the environment variable called pgpassword.

Now, one thing that's a little bit unclear here,

or I feel like might be a little bit confusing,

is the fact that our name of the environment variable

and the key inside the secret are identical.

So again, we could have very easily changed,

say, the key right here to be

mypassword without any issue whatsoever.

We would have just needed to make sure that

the key that we provided right here,

rather than pgpassword,

it would've had to have been mypassword as well.

All right, so I'm gonna change that back, like so.

Okay, so that's how we wire up

a secret as an environment variable.

So our server now knows about the password

to use for the database.

So now, the last thing we have to do,

is to make sure that our Postgres database

knows about the password that it should be using.

So we're essentially going to override its default password.

All right, so for this, we're gonna find our

Postgres deployment file.

We're gonna scroll on down

to our container definition right here,

and we're going to add on a

ENV property to our container definition, like so.

Now, please triple check, make sure that ENV

is on this same indentation level

as the name image ports and volume mounts properties.

If you wanna be real safe,

you could actually just add it up right here,

and not have to worry about matching the indentation at all.

Just a quick reminder, all these key value pairs

inside of a YAML file do not need to be ordered

in any specific fashion.

Okay, so for ENV, right here,

I'm gonna pass in a name of pgpassword.

So now we are setting up a environment variable

of pgpassword that we're going to pass into the container.

If the container or the image Postgres right here

sees an environment variable of pgpassword,

it's going to use that as the default password,

as opposed to the...

Or as the password, as opposed to

the default password of whatever it usually is.

And again, I forget what it is off the top of my head.

Okay, so we're going to create the pgpassword

environment variable,

and then we're gonna say,

you're going to get the value for this from

a secret key reference.

And again, it's going to have a name of pgpassword.

And the key that we want to reference inside

of that password, or that secret,

is pgpassword right there.

So we will provide a key of pgpassword like so.

All right, so that's pretty much it.

So we have now set up a custom password for our database.

We have told our copy of Postgres to make use

of that password, anytime someone tries to connect.

And we've also told our server pod,

or more specifically the multi server container

that gets created, what that secret password is.

So that's pretty much it.

Now, the very last thing that we are going to

need to do is to apply the changes that we just made

to these configuration files.

So I'm sure you recall how to do that.

We'll flip on over to our terminal,

and we'll do a kubectl apply dash F K8s, like so.

Now it looks like I've got a little bit

of an error message here.

You'll notice that it says,

"Cannot convert N64 into string."

I know exactly where that's coming from.

This was a expected error, don't worry about it.

So we'll take a quick pause right here.

When we come back the next section,

we're gonna fix this thing up very quickly.