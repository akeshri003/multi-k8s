![[Screenshot 2026-04-20 at 11.41.03 AM.png]]![[Screenshot 2026-04-20 at 11.41.14 AM.png]]
![[Screenshot 2026-04-20 at 11.41.39 AM.png]]

-: In the last section

we tried to apply our updated configuration files,

but we very quickly got an error message

that said something like cannot convert N64 into string.

So this is an error message

that I personally just wanted you to see,

because I can almost guarantee you'll see this

when you start putting together your own application.

We're seeing this message right here,

because inside of our server deployment file

and our worker as well,

we provided some environment variables as integers.

So right here, our redis port has a integer value

or essentially a number as 6379

and the PG port is 5432, also a number.

Whenever we provide a environment variable

we have to provide it as a string.

So for the value of 6379

all we have to do is wrap this with a quote like so,

and the same thing on the PG port as well.

That's all we have to do.

Then inside of our worker deployment

we'll find the redis port of 6379.

Again, we cannot have an environment variable

in here as a number.

We have to provide it as a string

and so, the simple work around is to just wrap this thing

with a set of quotes like so.

All right, now that we've made those changes

I'm gonna save both those files

and then we'll flip back over to our terminal

and try to apply these config files again.

So I'll do a kubectl apply /fk8s.

Now I'm gonna tell you if we get any errors this time

I was not planning on it.

So if we see an error message,

we're gonna have to go back and figure out what's going on

and try to fix it.

All right, so I'm gonna run that and it looks like

everything worked out just fine.

You'll notice that the server deployment

has been configured.

It looks like our worker deployment

has also been configured.

Now I'm looking for Postgres in here.

Where's our Postgres deployment?

It looks like the Postgres deployment is unchanged

which is not quite expected.

However, you know what probably happened

is it probably applied the config

during our last apply in the last section.

So I think it's totally fine to see

that the Postgres deployment is unchanged there.

Unfortunately, I've already cleared my console log

so I can't scroll up

and verify that Postgres did get configured successfully.

Nonetheless, we'll be able to test this out soon enough

inside of our browser

and be able to verify that everything worked correctly.

So let's take a quick pause right here

and we'll continue in the next section.

There's just one last little piece of configuration

that we have to do to set up our application.