> No Resources Found?
> 
> If you have deployed your issuer and certificate manifests to GCP and you are getting **No Resources Found** when running `kubectl get certificates`, then continue on to the next lecture to create and deploy the Ingress manifest. Deploying the updated Ingress should trigger the certificate to be issued.

> Required Update for the HTTPS Ingress
> 
> In the upcoming lecture, we need to make one small change to one of the annotations:
> 
> `certmanager.k8s.io/cluster-issuer: "letsencrypt-prod"`
> 
> change to:
> 
> `**cert-manager.io**/cluster-issuer: "letsencrypt-prod"`


![[Pasted image 20260423005538.png]]
![[Pasted image 20260423005600.png]]
![[Pasted image 20260423005641.png]]
![[Pasted image 20260423005810.png]]
![[Pasted image 20260423005820.png]]
![[Pasted image 20260423005906.png]]
![[Pasted image 20260423010007.png]]
-: Last thing we have to do to set up HTTPS traffic

with our cluster is to reconfigure our NGINX Ingress.

Inside my code editor,

I'm gonna find the Ingress service config file.

So we're gonna make a couple changes to this file

just to tell it, "Yep, here's how things are working out.

You are now serving up HTTPS traffic."

The first change that we're going to make

is going to be to update the annotation section.

Underneath the existing two annotations,

we're going to add on a rule

of certmanager.k8s.io/clusterissuer is letsencrypt prod.

So this little piece of configuration right here

is just going to inform our Ingress service

that we are going to be making use

of a Let's Encrypt certificate.

And you know, I'm looking at this now,

and I noticed I just made a small typo.

It should be letsencrypt-prod like so.

Okay, so again, this is gonna make a little change

to the NGINX server and it's going to essentially tell it,

"Hey, you should be expecting to get a certificate

from this issuer,

the issuer designated by let's encrypt prod."

The next thing that we're going to do

is we're going to make sure that the NGINX server

always forces users to make use of HTTPS traffic.

So we do not want any users accidentally

going to the HTTP version of our website,

because that's not a secure connection.

So to make sure that our users always get redirected over,

we're going to add on another annotation here

of nginx.ingress.kubernetes.io/ssl

redirect is going to be true.

And remember True goes into a string,

because the value true is interpreted differently

inside of a YAML file.

So like I said, this is going to reconfigure NGINX,

and tell it to always make sure that if someone

is trying to access our service,

they must be using a HTTPS connection.

So now the next thing we're going to do

is to update our spec section down here,

and tell NGINX that it should be serving up HTTPS traffic,

and also tell it where it can get our certificate from.

So to set up the initial HTTPS side of things,

under spec I'm going to add on a TLS section.

I'll then put down a dash and I'll say hosts.

And then another dash 'cause this is going to be a list

of different hosts that users can connect on.

And I'll put in W or say k8smulti.com,

and www.k8smulti.com.

And then on the same level as the host indentation,

I'll put down a secret name.

Notice how this does not have a dash in front of it.

And then the name that we're going to put in here

is the name of the secret that we had stored

our certificate in.

So to get that, I'm gonna go back over

to my certificate.yml file.

Here's the secret name that I used, k8s-multi.com.

So back over here, I'll do k8s-multi.com, like so.

Oh, it looks like I might have made a little typo there.

So to correctly create the YAML array,

looks like we wanted two indentations off of hosts.

I apologize for that.

All right, so again, this is going to tell NGINX

that we want to serve up HTTPS traffic.

We want it to be served up on

these two different host names,

and the certificate or the secret

that holds the certificate to allow for HTTPS traffic

is served or stored inside the secret of name k8smulti.com.

Now the last thing we have to do is a little bit

of a reconfiguration of our rules down here.

And I'm gonna tell you right now

that the little reconfiguration we're going

to do is gonna be a little bit over the top.

Unfortunately, this is stuff that is,

just has to be done this way,

because of the way that the NGINX Ingress

is currently written.

So hopefully at some point in the future,

we don't have to do this little reformatting

that you're gonna see,

but at least for right now it's just what we have to do.

Okay, so on the real section,

I'm gonna put a new line right above HTTP like so,

and then I'll designate a host of k8s-multi.com.

So this is essentially saying that

if someone comes to k8smulti.com,

here is the set of rules that we want

to be applied to that request.

Now, the part that's funky about this

is that if a user comes to you at www.k8smulti.com,

the entire rule set that we have right here,

this does not apply to someone coming to WWW.

WWW is recognized as a separate host

from the host name that we listed right here

of k8smulti.com.

So essentially, if someone comes in on this address,

great, we've got a set of rules to be applied to.

However, if someone comes in on this address,

the same exact rule sets do not apply.

So the part about this that is funky

is that we essentially have to take

this entire block right here and copy-paste it down.

We have to duplicate this entire block.

And then on the second block,

we're going to change the host to be www.k8s.

So I'm going to do my copy and then I'll paste.

So here's the second block right here.

I've got block one, block two,

and the only thing we need to change in block two

is to add on the WWW like so.

Again, this is a really unfortunate part of NGINX Ingress.

You can look on the GitHub issues board,

and see a couple threads around this issue,

because right now, essentially,

yeah, we've gotta list out the two separate hosts,

and it's just a little bit...

It doesn't feel very dry, I guess is what I'm saying.

Right?

Don't repeat yourself.

Well, we're definitely repeating the heck out

of ourselves here.

All right, so that's pretty much it.

So let's now review what's going to happen.

We made changes to the Ingress service config file.

So when we deploy this thing or apply it to our cluster,

the Ingress controller is going

to see a change our Ingress resource.

It's then going to create a new NGINX config file

out of everything we put inside of here,

and then reload the NGINX pod with this new config

that should start serving up HTTPS traffic.

So last thing we have to do is save the file.

I'll go back over to my terminal,

I'll do a git add,

I'll do a commit and I'll say updated Ingress,

and then I'll push to Origin Master again.

All right, that's pretty much it.

So now, again, we're gonna sit around,

and wait for a couple minutes.

After this deploy goes in,

we should then eventually be able to test out our browser

going to HTTPS,

your domain name,

and we should see our page load

without any error message like this right here.