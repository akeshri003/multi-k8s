![[Pasted image 20260423002153.png]]
-: In the last section,

we installed cert manager into our Kubernetes cluster.

We're now gonna briefly discuss exactly

what we have to create to integrate cert manager

into our project and get it working the way we expect.

So a couple diagrams here.

Here we go.

So in this diagram at the very center is cert manager.

As you just saw when we installed it,

it created a deployment, which in turn created a pod.

There's also a service account

and a cluster roll binding in there as well

which essentially gave that pod the ability

to manipulate our cluster

and create different resources inside of it.

So cert manager by itself, you can really just think of

as essentially being a pod that is going to reach out

to our cluster and kind of mess around with things.

And then at some point in time,

it's gonna set up a route handler to respond

to the Let's Encrypt verification request.

Now, in order to get cert manager working the way

that it expects,

there are two different objects that we have to create

with two different config files inside of our project.

So these are two types of objects that we're gonna create

and the same way that we've created

many other objects before.

We'll talk about the issuer first

over here on the right hand side.

The issuer is going to be a config file

that's going to tell the cert manager how to reach out

to a certificate authority and obtain a certificate.

So in our case, we are using a certificate authority

or essentially an entity who issues certificates

of Let's Encrypt.

Let's Encrypt publishes some public APIs

and API addresses to access those

out for you to read very easily.

So we're going to create this issuer object

that says essentially we want to use Let's Encrypt

and here's the address to the API to use

to initiate this entire exchange process.

So that's the purpose of the issuer.

It's just telling cert manager where to go

to attempt to initiate this verification process.

The reason that we create this in an object

is that cert manager is set up

to allow you to define several different issuers.

And even if you're using Let's Encrypt,

you might want to have more than one issuer.

For example, with Let's Encrypt,

you get the ability to connect to a staging server

or essentially kind of a practice version of Let's Encrypt.

So for example, if you're not sure that this entire flow

is going to work the way that you expect,

you would access the staging version or essentially

the practice version of Let's Encrypt before eventually

moving over to the real production version

of Let's Encrypt

when you eventually want to get your certificate.

So that's why we were able to create multiple issuers.

Now for us, I'm pretty confident

that everything's gonna work correctly the first time maybe.

So we're not gonna create multiple issuers.

We're gonna create just the one,

and we're going to go directly to Let's Encrypt production

the first time around.

Honestly, if you use staging, that staging workaround

with NGINX ingress like we are,

life actually gets more confusing than you would think

so it's kind of better to just access the production version

of Let's Encrypt first time around,

rather than trying to use the staging version

and then flip over to production later on.

Now, the other object that we need to create again

with the config file

is going to be something called a certificate.

The certificate is going to have some information

that details the exact nature

of the certificate that we want to obtain.

So whenever you get a TLS certificate

such as the one I'm using, or the one that draw.io uses

which is this diagramming tool that I am using right now,

if you look at the certificate right here,

you'll see that it has some information

in the details section like who draw.io is.

So it looks like it's issued by CloudFlare.

There's some other information on here

about location and issued by blah, blah, blah.

Essentially, there's a lot of information

tied to the certificate.

So the purpose of this certificate object file

or the config file is to say

here is the domain name that we want to use

for this certificate.

It'll also have some information about maybe subdomain

so we want to register under the certificate as well.

And just in general, it's going to describe the nature

of the certificate that we want to get.

The certificate is also going to define a Kubernetes secret.

Remember, we use a secret to store secret information.

Now the certificate gets a little bit relevant later on

so let's continue walking through this flow.

So we're going to create the certificate and the issuer

and then we're going to not necessarily feed them

into the cert manager.

The cert manager is actually going to automatically see

that we have created a certificate and an issuer.

Once it sees that both of these have been created,

it's then going to initiate that exchange process

with Let's Encrypt.

Let's Encrypt is going to eventually issue a certificate.

So once that cert manager gets the certificate

from Let's Encrypt, it then needs to store it

somewhere inside of our Kubernetes cluster.

And that's the purpose of the secret

that got created over here by the certificate.

So just to be clear, you and I are not going

to create the secret in the same way that we did previously.

So we're not gonna run QCTL create secret

or anything like that.

We're going to instead put an entry

into the certificate configuration file

that says hey certificate, when you get created,

you should make a secret

called something, something, something.

And the secret will be created for us automatically.

Now, once we get that certificate

and stored inside the secret, the last thing

we'll have to do is reconfigure our NGINX ingress.

So we're going to make a little change

to that configuration file for our ingress

and we're going to tell it hey,

you should now start serving up HTTPS traffic.

And by the way, here is a secret

that is holding the certificate that you need to use.

So that's pretty much the overview of what we have to do.

We have to create this certificate, the issuer,

and then we have to do a little bit

of a reconfiguration on our NGINX ingress.

So let's take a quick pause right here.

When we come back in the next section, we'll get started

by working on our issuer config file to start.

So quick pause and I'll see you in just a minute.