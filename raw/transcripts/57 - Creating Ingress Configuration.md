![[Screenshot 2026-04-20 at 1.19.50 PM.png]]
![[Screenshot 2026-04-20 at 1.22.45 PM.png]]![[Screenshot 2026-04-20 at 1.23.40 PM.png]]
![[Screenshot 2026-04-20 at 1.28.50 PM.png]]
```ingress-service.yaml
apiVersion: v1

kind: Service

metadata:

name: ingress-service

annotations:

kubernetes.io/ingress.class: nginx # what platform or service provider are we using for ingress/

nginx.ingress.kubernetes.io/rewrite-target: / # how the actual copy of nginx behaves

spec:

rules:

- http:

paths:

- path: /

backend:

serviceName: client-cluster-ip-service

servicePort: 3000

- path: /api/

backend:

serviceName: server-cluster-ip-service

servicePort: 5000
```
-: In the last section, we set

up ingress-NGINX on our local machine.

Now the last thing we have to do is create a

new config file that's going to specify the different sets

of routing rules

that we want to use, inside of our application.

We're going to use the exact same routing rules

that we had to use back on some of our previous projects

specifically the Elastic Beanstalk one

where we set up our own custom NGINX server.

So if you recall back on that project, we had said

that anytime we had some amount of incoming traffic

we looked at the path of the request

and we had said that if the request started with slash API

we were going to automatically send that request

off to the server.

Otherwise, we would send the request

off to the client assuming

that it needed to access some of the client side resources.

So we're going to set up a ingress config file

right now that's going to implement this routing rule set.

So inside of my code editor

I'm gonna find my K8s directory, and inside there

I'm gonna make a new file called ingresservice.yaml.

Then inside of here

we're gonna to write out our config file.

That as usual, is gonna look very similar to a lot

of the other config files we've already put together.

So I'm gonna specify an API version.

This one is going to have a longer version

on it that we've not used before.

We'll say extensions B1 beta one.

The kind of object we are gonna create is an ingress.

We'll then set up our metadata.

So we'll give it a name of ingress service,

and then unlike previous metadata fields that we set up

this one is actually gonna have a couple

of other metadata properties as well.

So I'm gonna add on a new thing to this called annotations.

Annotations are essentially additional configuration

options that are going to specify a little bit kind

of higher level configuration

around the ingress object that it gets created.

So the first thing we're gonna do here, we're gonna add

on kubernetes.io/ingress.class of NGINX like so.

So this configuration rule right here is essentially

telling Kubernetes that we want to create a ingress

controller based on the NGINX project.

Then immediately after that, we're going to add

in another configuration rule that's going to specifically

configure how our copy of NGINX behaves.

So I'm gonna say

NGINX.ingress.kubernetes.io/rewrite target/ like so.

So this additional annotation right here, again

this is going to configure how the actual copy

of NGINX behaves.

This rule in particular says that if we end

up matching a route like slash API right here

after deciding to send it to the server, that configuration

is gonna first do a little bit of a rewrite on the request

and essentially it's going to remove the slash API part.

Now again

we did a step like this, very similar back on

the Elastic Beanstalk project that we put together.

We had said

that we probably did not want to put some configuration

inside of our server that very tightly coupled it

to the routing that got request

to the server in the first place.

So we had decided to remove these slash API

outta the incoming request just so

that we did not have to write slash API

on every different route on the server itself.

Okay, so that's exactly what this line

of configuration is doing right here.

So now after that we'll un indent and we'll add in a spec.

So for the spec we're gonna add a series of different rules.

In our case, we're gonna have just one rule.

So I'm gonna put in a single dash like so

and then I'll say HTTP paths.

This is going to be an array as well.

So I'll do a dash

and I'll say path slash backend is going to be service name

of our cluster client cluster IP service, which has a name

of client cluster IP service

and a service port of 3000.

I'll then add in another path here

of slash API slash I'll give it a backend as well.

This is going to have a backend that points at the

where is it?

Server cluster IP service right here, which has a name

of server cluster IP service,

and a service port of 5,000.

Okay, so let's talk about exactly what's happening here.

Now we discuss the annotations

and then we very quickly dove into all the different rules.

So our rules are saying that there are two

possible paths that we can match traffic to.

If someone ever comes to the path of just slash

by itself or any route that looks like slash followed

by anything besides slash API, then we want to

send that request to whatever set of pods are governed

by the service of client cluster IP service.

And again, we just referred to the name

of that other service as we had designated it inside

of our client cluster IP service file.

Then if a request came in with a leading route name

of slash API, we wanted to to instead send it to the set of

of pods that are governed or managed by the server

cluster IP service.

So essentially we're setting up the exact same relationship

that you just saw in this diagram right here.

The NGINX configuration that we're putting together

is going to send all this incoming traffic to

either the multi-client set of pods governed

by this cluster IP service or the multi server set

of pods governed by this cluster IP service.

We don't have to actually specify the IP address

of these servers inside our cluster.

We just refer to the name of the service and then

all this NGINX ingress stuff is going to figure it out

from there.

You'll notice that we also had to specify the port

for both these services as well.

Okay, So that's pretty much it for this config file.

Now, the description that I've given you

of this config file is pretty light

at this point in time, like I haven't really told you

about why we have multiple rules

in here or this http flag or stuff like that.

We are going to actually come back to this file

over time and make a couple of additions to it over time.

So don't we worry we are gonna come back

to this file and have some further discussion

about what's going on inside of here.

Now last thing I wanna do is take this file

and apply it with Cube ctl, which should set

up some networking inside of our application.

So I'm gonna flip on over to my terminal

and I'm going to do a cube CTL apply dash F

and then as usual

we'll just apply everything inside of the K8s directory.

So I'm gonna run that and somewhere inside of here

we should see ingress service created right there.

So let's take a quick pause right here.

When we come back to the next section

we're gonna do a little bit of work to make sure

that the ingress is up and running as we would expect.

So quick pause and I'll see you in just a minute.