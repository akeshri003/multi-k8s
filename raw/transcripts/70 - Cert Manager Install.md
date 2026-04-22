Required Updates for Cert Manager Install

In the upcoming lecture, we will be installing the Cert Manager using Helm on Google Cloud. There have been some breaking changes introduced with the latest versions of Cert Manager, so we will need to do a few things differently.

Instead of the installation instructions given at around [1:20 in the video](https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide/learn/lecture/11628348#questions), we will complete these steps in the GCP Cloud Shell:

1. Add the Jetstack Helm repository
    
    `helm repo add jetstack https://charts.jetstack.io`
    
2. Update your local Helm chart repository cache:
    
    `helm repo update`
    
3. Install the cert-manager Helm chart:
    
    1. helm install \
    2.   cert-manager jetstack/cert-manager \
    3.   --namespace cert-manager \
    4.   --create-namespace \
    5.   --version v1.8.0 \
    6.   --set installCRDs=true
    

Official docs for reference:

[https://cert-manager.io/docs/installation/helm/#steps](https://cert-manager.io/docs/installation/helm/#steps)


-: Now that we've got our domain purchased and configured,

we're going to start to create all the infrastructure

that's going to facilitate the automatic communication

between our cluster and Let's Encrypt

to eventually obtain a TLS certificate.

Now the project that we're going to be using to do this

is called Cert-Manager, that's the name of the project.

The homepage for this project is at

github.com/jetstack/cert-manager.

We're going to go there right now

and follow their documentation to install Cert-Manager

into our cluster.

So inside of a new browser tab

I'll go to github.com/jetstack/cert-manager.

And then once here, I'll scroll down a little bit

and find the documentation section.

So here's the link to the documentation right here.

Once here, I can find the Getting Started link

on the left hand side,

and then I'll find Installing Cert-Manager with Helm.

So I'll click on that and there we go,

that's the command we need to run.

Now, we haven't really said what Cert-Manager

is doing for us just yet.

We are going to have a discussion

on what Cert-Manager is doing,

and how we actually integrate it into our project.

But for right now, I just want to get it installed.

So I'm going to copy this link right here.

I'll go back over to my Kubernetes dashboard

on Google Cloud, where I've still got my cloud Shell open.

So I'm gonna paste in that command and run it.

That's gonna download a bunch of config files

and install them inside of our Kubernetes cluster

as a couple of different new objects.

Now, you'll see that there's a little bit of information

in here, it looks like it created a new pod for us,

a new deployment, new cluster roles, and some other stuff

in here like a service manager, and whatnot.

So this is looking pretty good,

everything is definitely set up correctly.

So let's take a quick pause right here.

When we come back to the next section,

we're gonna start talking about exactly

what Cert-Manager is doing for us,

and how we integrate it into our project.

So quick break and I'll see you in just a minute.