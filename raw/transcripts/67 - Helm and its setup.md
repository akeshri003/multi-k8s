Important - Updated Ingress Installation with Helm

In the next lecture, we will be installing Ingress Nginx using Helm. You will need to run an updated command instead of what is shown in the video.

In your Google Cloud Console run the following:

  

1. helm upgrade --install ingress-nginx ingress-nginx \
2.   --repo https://kubernetes.github.io/ingress-nginx \
3.   --namespace ingress-nginx --create-namespace

  

Check that your Ingress Controller was deployed:

`kubectl get service ingress-nginx-controller --namespace=ingress-nginx`

  

**Link to the docs:**

[https://kubernetes.github.io/ingress-nginx/deploy/#using-helm](https://kubernetes.github.io/ingress-nginx/deploy/#using-helm)


-: There's one last piece of configuration that we have

to set up on our production Kubernetes cluster

before we can actually deploy our application.

If you open up your Code Editor right now,

and find your k8s directory,

you'll recall that we had set up that ingress service.

And the ingress service relied upon

the ingress nginx project

which we had installed into our local mini-cube cluster

through the use of that mini-cube add ons enable

ingress command.

It was specifically that command that allowed us

to use this nginx ingress

into our local cluster.

Now we have to go through a very similar set up

on our production Kubernetes instance as well.

So by default, the cluster that we just created right here

has absolutely no idea of what an nginx ingress is

and we have to install it as a separate service.

Now as a quick reminder,

when we install that ingress we're going to get a couple

of things created for us.

First off, we've got the ingress config,

which is that ingress config file

we've already created inside of our project,

but we have to go through that additional setup

that is gonna create the load balancer service,

map it to a Google Cloud Load Balancer

and then also set up a deployment running the ingress

controller and the actual nginx pod that's gonna

do the real routing.

So this is all some additional setup

or some additional installation process

above and beyond everything that we've already done.

Now to get the setup directions for this,

we can go back to the documentation for the ingress

nginx page.

So the URL for that right here.

Remember, we do not want Kubernetes ingress.

We want ingress nginx.

And so I'm going to copy this URL right here,

github.com/kubernetes/ingress.nginx.

And I'll open that in a new tab inside my browser.

Now at the very top of this github repository

is the home page for this project,

so I'm gonna go there right now

and then at the top of this page is a section

marked as deployment,

which has directions on deploying this project out to

either a local or production environment.

So I'll go to deployment

and then inside of here you'll recall that there's both

the mandatory command and the provider specific commands

on here as well.

So if you wanted to we could use this mandatory command.

But we're gonna do things a little bit differently

this time around.

If you go back up to the top here and look at contents,

you'll notice that down at the bottom,

the very last option is using Helm.

So let's talk about exactly what Helm is.

I'm gonna click on that link.

And it says right here that the controller

can be installed using Helm.

So what's Helm?

Well, really straight forward,

Helm is essentially a program

that we can use to administer third party software

inside of our Kubernetes cluster.

So just as we've been talking about,

yeah we want to get some outside software

into our cluster, like this outside deployment

and the pod that it controls.

We want to install that as a third party system essentially,

and to do so we can either manually run all those apply

commands with all those configs that are mapped

up to them that we just saw inside the official

documentation over here.

So like all these apply commands that we've seen,

but we can also make use of this command line tool

called Helm.

Helm is very commonly used with some of these more

complicated projects where some of the setup

might be a little bit challenging in nature.

Now you might be thinking that there's nothing wrong

whatsoever with using this very simple apply command

right here that we had ran previously,

and using the very simple apply command

with GCE right here,

those look like they're really straightforward,

but in this case we're gonna use Helm instead,

because something that we're gonna do a little bit

later on is going to absolutely, positively

require us to use Helm.

So this is a pretty good opportunity to get this thing

set up and understand exactly how it works.

So to get started, I'm going to navigate to

github.com/helm/helm.

Oops.

Helm, there we go.

And then on this page we're going to go to the official

documentation, the link is right up here at the top.

But do note that there is other information

inside this read me right here

that you might want to be aware of.

Okay so we'll go to helm.sh

and then on the top right-hand side, we'll find get helm.

Oh, I forgot about that.

It just links you back to the github page.

Okay we're gonna try this a different way.

We're gonna go down to the read me

and down here it's gonna talk about install

and we're gonna go to the quick start guide

right there, that's better.

All right that's what we're looking for.

Okay so on this page, we're going to go through

one of these install directions.

Now there's a lot of different ways of installing Helm

because it assumes that you might be running Helm

in production or you might be running it on your

local computer.

So one of the preferred ways that it lists inside of here

to install Helm is it's gonna say, oh yeah,

you should install using Home Brew, which is a tool

for installing software on a MacOS machine.

And clearly we probably do not have access to Home Brew

in our production Kubernetes environment.

So we cannot use this preferred method.

So instead we're going to scroll down a little bit

or maybe a lot,

we're gonna go down, down, down,

so here's the installing Helm section.

You could also get here by clicking the link

on the left-hand side.

And then we'll go a little bit lower here,

a little bit lower, a little bit lower, there we go.

So From Script, this is what we want to do.

If you can't find this section, you can of course

just do a control F and search for

From Script like so.

So to install Helm we're going to copy these three

commands right here and run them inside of our Cloud

Console on Google Cloud.

So I'm gonna take the first command right here,

not the dollar sign, I want just the curl.

I'm gonna copy that.

And I'll paste it over here inside of my Cloud Shell.

That's gonna download that script.

We then need to change the permissions

for the script that we downloaded by running change mod.

So I'll copy that.

We'll run it as well,

and then finally we'll execute the script with

./get_helm.sh.

Okay so that's gonna do a little bit of setup

and then very quickly you'll see,

okay everything's good to go.