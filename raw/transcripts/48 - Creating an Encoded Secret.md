done![[Screenshot 2026-04-20 at 11.09.32 AM.png]]
![[Screenshot 2026-04-20 at 11.12.22 AM.png]]![[Screenshot 2026-04-20 at 11.27.47 AM.png]]
-: There's one last environment variable

that we have to set up the PG password.

This environment variable is a password

that's going to be used to get access to our database.

Now, one thing I want to be really clear about right now,

is that the password is tied to Postgres itself.

This is not a password related

to Docker or Kubernetes or anything like that.

We are talking about just getting access to Postgres.

We need to make sure

that we add in this environment variable

and pass it to the multi-server.

But like I said in the last video, chances are

that we do not want to write out our password

in plain text and store inside of our config file.

So rather than adding in our password as plain text

or anything like that, we're going to use a new type

of object inside of Kubernetes

that we have not yet discussed.

So the new type of object we're going to use

is something called a secret.

So it's a very similar type of object

in the sense that we had previously created a pod,

a deployment, a service, and so on.

The purpose of a secret is to securely store one

or more pieces of information inside of your cluster.

So a good example of a use of a secret would be

for a database password, an API key,

or maybe an SSH key, or any similar type

of secret piece of information that you do not

want to have easily exposed to the outside world,

but you do want to make available

to run in containers inside of your application.

And so our database password perfectly qualifies

as something that we would want to store inside a secret.

Now, the secret is only about information

that you want to very much protect.

So in our example of our environment variables here,

storing the reddest host right here would've not

been a very good use of a secret,

because the reddest host is always going to be the name

of our service, and the name of our service

is very plainly legible and readable inside

of our reddest clusterIP file right here.

So not a good reason to try to store the reddest host

for example, inside of a secret

probably saying for the thing for the port.

Now, maybe the PG user would be something

to store inside of a secret.

Maybe the PG database would be as well.

But for our example here,

we're just gonna worry about encoding our PG password

and storing it inside of a secret.

All right, so how do we create a secret?

Well, a secret is an object very much

like a pod, a deployment, a service.

And so we have said a billion times

throughout this course that we always create an object

through the use of a config file.

However, you and I are not going

to use a config file to create a secret.

Instead, we are going to run a imperative command

that's going to create the secret for us.

So why are we using an imperative command here

as opposed to a config file?

Well, when you create the secret,

you have to provide the data that you want to encode.

And so if we had to write out a config file that said,

hey, here's our secret, like in plain text

here's the PG password, then it would kind

of defeat the purpose of having a secret anyways.

So we're going to create our secret with a single command

that's gonna be imperative in nature,

and that means that we're going to have to run

this command locally on our computer to create the secret.

It also means that when we eventually take our application

to a production environment,

we're going to have to make sure that we create

the secret manually in the production environment as well.

Everything else that we've done with the config file,

such as our deployments and our services,

do not need to be manually created

in a production environment.

All we have to do is apply the config files

that represent each of them.

So again, our secret being imperative in nature,

we're going to have to make sure that we manually create

this thing in any environment that we eventually

take our application to.

So to create the secret,

we're going to run a rather long kubeCTL command.

I'm gonna zoom in on the thing

and then we'll go over it step by step.

So we're going to use kubeCTL Create.

Create is a imperative command that we can use to create

a new object without necessarily using a config file.

You will see a lot of documentation and a lot

of blog posts out there talking about kubeCTL Create.

Remember, we have not been using this command

throughout this course, because we've been taking

the more imperative, or excuse me, more declarative approach

of using those config files.

We're then going to designate the type

of object that we want to create.

So of course we want to make a secret,

and then we will designate the type of secret.

Now, our type of secret is generic,

which indicates that we are saving some arbitrary number

of key value pairs together.

There are two other types of secrets that you might create.

The first one is a Docker registry.

So you would put in Docker registry.

The other type of secret you can make is TLS.

And TLS is related to say, HTTPS setup.

And we would use the TLS right here

to store a set of TLS keys.

Now, the vast majority of the time

you are going to use the generic.

We would use the Docker registry one anytime

that we want to set up some type of authentication

with a custom Docker registry that would,

we would be storing our images in.

Now, you and I are storing all of our images

in Docker Hubs, so no custom authentication is required.

So we do not need to make a secret of type Docker registry.

And then as far as that TLS secret, we actually

might be setting up some HTTPS stuff later.

So we might end up using that other type of secret

at some point in time, but we'll see.

Okay, so after generic, we'll then put

down a name for the secret.

The secret name is very important.

It's very similar to the name property.

So we've added to all of our other objects.

The name property is going to be how we refer back

to the secret at some point in time in the future when

we actually want to consume it and make use

of this secret inside of a pod config.

Then the last two arguments

on here are going to be from literal.

So when we use dash dash from literal,

that means that we're going to write out

the information to be stored inside the secret

in this actual command, as opposed

to trying to write the secret information into a file

and then load up all that secret data from the file.

So because we are using from literal,

we're going to add on our information as a key value pair

at the very end of the command.

You and I want to encode some environment

variable called PG password.

So we'll probably end up doing something

like PG password equals and then our actual password,

whatever it might be.

You know, for us, we'll probably keep it something simple,

like password123 or whatever it might be.

Okay, so that's the entire command.

So let's flip over to our terminal.

We're gonna write this thing out.

And again, I want to remind you

that we're gonna have to run this command locally

in our machine and when we eventually

move off to production, we're gonna have to run

the same command again to create a secret

on that new environment as well.

So let's get to it.

I'm gonna flip back over to my terminal.

I'm going to run kubeCTL, create secret.

We're going to make a type of secret called generic.

The secret name will be PG password.

Notice how we're doing this in lowercase here.

So this is not the actual environment variable.

It's not the actual secret,

it's just the name of the secret.

So we're going to eventually have to reference

this thing in the future and say,

hey, go pull some information

outta the secret called PG password.

We'll then do dash dash from literal.

I'll zoom in here just to make sure it's nice and legible.

There we go.

So dash dash from literal, and then we're going to put

down the actual key value pair that we want to encode.

So I'll do PG password equals, I don't know

one, two, three, four, five, ASDF,

whatever you want it to be.

It's totally up to you.

All right, so that's the entire command.

So I'm now going to run this,

and we'll see that the secret was created.

And now just to verify that, yep,

it was successfully created.

And to look at the secret, we'll put

in kubeCTL get secrets, like so.

And it's gonna tell us,

all right, we've got a secret called PG password

and there's one key value pair inside there.

And the key value pair is the PG password

that we put in right here.

All right, so that's it.

We've now created a secret that is going

to store our Postgres database password.

So now the last thing we have to do is wire

up the secret to our server deployment.

So we need to make sure that our server container knows

about this secret and receives it as an environment

variable, and then the other thing we have

to do, that might be a little bit less obvious,

is that we have now created a database password

that is something different than the default

Postgres password, which is by default,

something like, I don't know,

PG password or whatever it is.

I forget it off the top of my head.

So we need to make sure that we also update our

Postgres deployment as well.

We're going to update our Postgres container definition

and tell it about the password

that it should be using as its default check.

So we want to override the images default password

that it sets up when it creates a copy

of database inside that container and say,

hey, use this password instead.

And so anytime that someone tries to connect to the database

it's going to try to authenticate that connection

with the password that we provide to it.

So two places to wire up this secret.