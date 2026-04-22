> Required Update for the Certificate
> 
> In the upcoming lecture, a few minor changes are required per the [official docs](https://cert-manager.io/docs/tutorials/acme/http-validation/#issuing-an-acme-certificate-using-http-validation):
> 
> 1. Update the API version used:
> 
> `apiVersion: cert-manager.io/v1`
> 
> 2. Remove the acme challenge from the certificate spec.
> 
> The full updated Certificate manifest can be found below:
> 
> 3. apiVersion: cert-manager.io/v1
> 
> 4. kind: Certificate
> 5. metadata:
> 6.   name: yourdomain-com-tls
> 7. spec:
> 8.   secretName: yourdomain-com
> 9.   issuerRef:
> 10.     name: letsencrypt-prod
> 11.     kind: ClusterIssuer
> 12.   commonName: yourdomain.com
> 13.   dnsNames:
> 14.     - yourdomain.com
> 15.     - www.yourdomain.com
> 
>   
> 
> QA thread for reference with some troubleshooting steps (see Joseph's post near the bottom)
> 
> [https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide/learn/lecture/11628364#questions/8558842/](https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide/learn/lecture/11628364#questions/8558842/)


![[Pasted image 20260423004252.png]]
![[Pasted image 20260423004503.png]]
-: In this section we're gonna start putting together

our certificate file.

Again, this is a, going to be a config file

that's going to describe some details

around the certificate that we are trying to obtain.

So back inside of my code editor

I'm gonna create a new config file

inside the K eights's directory

called certificate dot YAML.

Then inside of here, we'll put together

a little bit of config.

As usual, looks very similar

to a lot of the other config files we've put together.

We'll start off with an API version again

of cert manager dot K eight dot io slash v one alpha one.

The kind is going to be a certificate

and our metadata is going to have a name

and I'll use my domain name in here just to

make sure that's really clear what this certificate is for.

So my domain was K eight's multicom,

and I'll put on TLS on here

just to indicate that this is a TLS certificate.

All right. After that we'll put down our spec.

We're going to provide a secret name of keightsmulti.com.

You can replace this with whatever your domain name is

both for the name up here

and for the secret name down here as well.

The secret name right here is specifying where

our certificate should be stored

after is obtained by Cert Manager.

So this is this little secret that is

created as a part of the certificate.

Again, we do not have to create the secret ahead of time.

It'll be automatically created for us by Cert Manager

once it obtains the secret.

After that, we'll define a issuer ref.

The issuer ref, is a reference to the issuer

that we set up and want to use

in order to obtain the certificate.

So for us our certificate issuer is going to be

lets encrypt prod.

So for issuer ref, I'll provide a name of lets encrypt prod

and then we'll also designate a kind on here as well.

The kind is just indicating the type

of issuer that's being used.

Our type of issuer is a cluster issuer.

So my kind back over here will be cluster issuer.

Okay, so that's the boring part.

Now here comes the more interesting stuff.

We're going to put down a common name.

My common name is going to be keightsmulti.com.

So for you, it's going to be just your domain name

with the TLD on there as well.

The common name is what's going to be put on the certificate

in bold letters saying,

this certificate is good for any address of keightsmulti.com

or whatever your domain name is.

So again, if you say, go back over to any existing webpage

that you might have access to and look at certificate

and then look at the details on

right there underneath details

it lists out a common name in this case of draw.io.

And so the common name in this case is saying,

yep this is what this certificate is good for.

Now after that we'll provide DNS names

and we're gonna put down two separate entries here.

The first entry is going to be your domain name

and then the second one will be the www version,

like so

remember this is in array, so get the two dashes on there

and make sure you've got the correct indentation as well.

Now we're gonna not talk about DNS names quite yet.

We're gonna fill out just a couple more lines

and we'll talk about exactly what DNS names means.

All right, so after that we'll put down ACME,

we'll put down config.

I'm gonna put a dash in here

and then say HTTP oh one,

a colon

and then indented inside there,

I'll say ingress class is NgenX.

And then underneath that we'll put domains

and I'll put down www.

Alright, so let's do it in order.

Same order as DNS.

So I'll put domains, a dash because this is an array

and I'll do keightsmulti.com.

So your domain in this case, of course,

and www dot your domain again.

So for me, keightsmulti.com, like so.

All right, so what's going on here?

We've got do DNS names out here

and domain names down here as well.

So what's going on is essentially DNS names is the list

of all the different domains that should be associated

with the certificate.

In other words, when we get back a certificate

it's going to be good

for a domain name of keightsmulti.com.

And it's also going to be good

for a domain name of www.keightsmulti.com.

So if a user goes to either of these addresses

our certificate is going to cover it.

Now down here we list out the same two domains again.

These are the different domains

that the verification process,

so this entire back and forth flow,

is going to attempt to access

to make sure that we actually have access

to the listed domain names.

So it might seem like we are

just repeating the same thing twice.

Yeah, we definitely are

but that's how the authors put this thing together

so we get the two separate listings.

Okay, so that's pretty much it

for this certificate config file.

So I'm gonna make sure that I save this as well.

Now we're gonna take a quick break

and we come back to the next section.

We'll do one last little piece of configuration

and then we're going to deploy this thing

and test it out in production.