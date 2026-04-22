>Required Update for Issuer

> In the upcoming lecture, the Issuer manifest will need a few small changes per these docs:
> 
> [https://docs.cert-manager.io/en/latest/tasks/issuers/setup-acme/index.html#creating-a-basic-acme-issuer](https://docs.cert-manager.io/en/latest/tasks/issuers/setup-acme/index.html#creating-a-basic-acme-issuer)
> 
> 1. Update **apiVersion**:
> 
> `apiVersion: cert-manager.io/v1`
> 
> 2. Add a **solvers** property:
> 
> 3.     solvers:
> 4.       - http01:
> 5.           ingress:
> 6.             class: nginx
> 
> The full **issuer.yaml** manifest can be found below:
> 
> 1. apiVersion: cert-manager.io/v1
> 2. kind: ClusterIssuer
> 3. metadata:
> 4.   name: letsencrypt-prod
> 5. spec:
> 6.   acme:
> 7.     server: https://acme-v02.api.letsencrypt.org/directory
> 8.     email: "test@test.com"
> 9.     privateKeySecretRef:
> 10.       name: letsencrypt-prod
> 11.     solvers:
> 12.       - http01:
> 13.           ingress:
> 14.             class: nginx 

![[Pasted image 20260423003502.png]]
Instructor: In this section,

we're going to start putting together our config

for the issuer object.

This is going to be another configuration file, very much

like all the others we have previously put together.

So inside of my code editor,

I'll find my K Eights directory, and inside there

I'm going to make a new file called issuer.yaml.

Again, the purpose of the issuer file is to describe

to our cert manager exactly where it should go

in attempt to get a certificate.

Now, just like many other config files we've put together

we're going to have an API version.

We're going to specify a API version here

very dissimilar to any other that we've put together so far.

We'll say certmanager.k eights.io/v1 alpha 1.

Now the reason for this very different

API version right here,

remember that the API version is essentially specifying

a bucket of different object types.

So we can pull from, we've said

that we're going to make an object of type issuer

and that is definitely not an object that is included

in Kubernetes by default.

It is something that is defined specifically

by the cert manager that we just installed.

So for the API version, we're saying

reach into the cert manager bucket of objects.

We want to be able to create a issuer type object.

So on the next line we'll define our kind of object

which is going to be a cluster issuer.

We'll then put down our metadata tag and give this a name.

We're going to name it after the source for the issuer.

So the purpose of this issuer is to tell our cert manager

to reach out to Let's Encrypt

and specifically the production version of Let's Encrypt.

So we're going to give this a name of

"Let's Encrypt - Prod", like so.

After that we'll put down a spec.

This is gonna have an entry of acme.

We'll then specify this server that Cert Manager

should reach out to when it tries to establish

this verification process.

So this is essentially a link to the Let's Encrypt API

that's going to be used for setting up

all this different communication back and forth.

So this is going to be a hard coded URL right here.

We're going to do "https", notice the S on there,

pull in "//acme-vo2.api.letsencrypt.org/directory", like so.

Now after that, we're going to put

in our personal email address.

Now the email is not actually used as part

of this verification or anything like that.

Let Encrypt says that they have to have your email

on hand just for whatever purposes they decide.

So we're not gonna actually get any emails or anything

like that or have to respond to anything.

Nonetheless, I still do recommend

that you put in your real email address.

So for my email inside of a string,

I'll put my email like so.

Next up, we're going to define something called

a private key secret ref

and we'll give it a value of name Let's Encrypt fraud.

All right, so the private key secret ref right here

is not at all connected to the secret that is generated

as a part of the certificate.

It turns out that

behind the scenes when your Kubernetes cluster

is going to eventually communicate with Let's Encrypt,

during this process at some stage, Let's Encrypt sends

over a little secret key that is tied to

essentially your record with Let's Encrypt.

And it actually comes into play

as a part of this verification process right here.

It's actually used to essentially decide exactly

how to respond when Let's Encrypt tries to access

this randomly generated URL.

It's this little key that gets sent back to us

and stored inside of the secret key ref

is not actually something we ever have to use.

So essentially we just kind of throw it on here

and forget about it.

Now the last last option we'll put on is HTTP01

and we're gonna put a set of curly braces like so.

Now this probably looks like a real weird option right here.

So all this option is doing is saying we want to

use this HTTP process of obtaining a certificate.

We want to reach out to Let's Encrypt and say,

'Hey, I want a certificate'.

Let's Encrypt says, 'Okay, respond on this route'.

And then we eventually do.

That's all this little configuration line right here

is doing.

There's actually another different way

of obtaining a certificate or going through

this verification process that is a little bit different

than the HTTP one that we're putting together right now.

But without a doubt, setting up the HTTP version

is a lot easier

than the other DNS option that is available.

All right, so that's pretty much it.

That's all we have to put together inside

of our issuer.yaml file.

So I'm going to save this file and I'll close it out.

We're gonna take a little pause

and then in the next section we're going to

start putting together our config file

for our certificate as well.