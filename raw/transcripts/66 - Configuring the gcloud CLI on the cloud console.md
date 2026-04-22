We're all done with our Travis.

YML file and our deployment script.

But before we try to deploy this, there's 1 or 2 little last pieces of configuration we have to put

together on Google Cloud.

Now I'm on my dashboard right here, and I'm making sure that I've got my multi k8's project selected.

Now let me give you a quick reminder of something that we did previously in our local environment that

we have to manually do on Kubernetes, on the cloud as well.

So one thing we had previously done was create a secret.

We had created a secret to make sure that the multi server had a PG password environment variable.

This environment variable of PG password was encoded into a Kubernetes secret and then shared with both

the multi server and our, excuse me, our Postgres image as well.

In fact, if you open up your code editor right now and find your K8's server deployment file at the

very bottom, you'll recall that we had hooked up our secret to this thing with a name of PG password

for the secret, and the secret contained a key of PG password as well.

So we need to make sure that we set up an environment variable or a secret, just like this one on our

remote Kubernetes instance as well.

So this cluster does not currently have a secret assigned to it.

For that pkg password, we have to manually create one by running that kubectl create secret command.

Now this is one of the very few instances of a imperative command that we have to run.

Remember, we did not create a config file for the secret, because that would kind of defeat the purpose

of using a secret in the first place.

The secret.

We specifically do not want to put that thing in plain text inside of our repository.

Okay, so all we have to do is essentially get access to a kubectl instance that is connected to the

multi-cluster hosted on Google Cloud.

Now, to do so, we do not have to reconfigure our local kubectl command or anything like that.

So we're not going to reconfigure this command or anything.

Instead, we're going to make use of a really awesome feature on Google Cloud.

So up at the top right hand side of your dashboard, I want you to find this little Activate Cloud Shell

button right here.

If you click it, you'll then very quickly be connected to a terminal or a shell that is essentially

running in the context of your Google Cloud project.

So inside of here, we can issue any set of commands against all of our production resources.

We can make use of kubectl and have this kubectl command apply to our multi-cluster that we just created.

And so we get to run all the same commands.

We had previously ran back on our local machine through this little terminal or the shell right here.

This is really one of the features that makes Google Cloud a lot easier to use, and a lot easier to

get started with than AWS.

Now, to be honest, with AWS, you can set up something similar by reconfiguring your local shell on

your computer, or you can SSH into one of your Kubernetes clusters that is hosted by AWS.

But the entire process of that is way, way, way more complicated than just pressing the button up

here and essentially being done with it.

Okay.

So we're going to run our kubectl create command right here.

Before we do we need to do just a tiny little bit of configuration.

So back inside of our YAML file remember how we installed the gcloud command right here at the Google

Cloud SDK.

And then we ran the series of commands right here to config and set our project and our compute zone,

and then eventually get a set of credentials for Multi-cluster as well.

We have to do the same thing on this shell over here as well.

So we need to set our project ID, we need to set our compute zone.

And we need to get those credentials too.

So to do so I'm going to first zoom this in a little bit so you can see it more easily.

We'll first run G cloud config set project.

And then we'll get our project ID by going up to the project dropdown up here.

And then finding the ID right here.

So I'm going to copy that and I will paste it.

Try it again.

There we go.

Paste it in.

So gcloud config set project and then the id okay.

So that looks good.

Now the next thing we need to do is set our compute zone.

Remember if you go to your Kubernetes Engine dashboard and you look at your cluster it'll print out

the location right here.

That is our compute zone.

So I'll do gcloud config set compute zone.

And then for me I'm going to use us Central1 a US central 1-A.

A.

Cool.

That looks good.

So now the last thing we need to do is that get credentials command.

So gcloud container clusters get credentials.

And then I'll put in the name of my cluster which is multi-cluster.

Cool.

So that's pretty much it.

Now just so you know, you only have to run this series of commands right here one time, unless you

create a different Kubernetes cluster at some point in the future, or a different project, every time

you create a separate cluster or a different project, you're going to have to rerun that series of

commands.

So in general, for the rest of this course, we're going to be okay.

But if you go off and start working on your own application at some point in the future with a new cluster,

you're going to have to run those again.

And as a very easy reference, you can just go back to the Travis file and reference these three commands

right here so that you know what to run in the future.

Okay.

So that is going to configure gcloud.

So we can now use kubectl safely inside the shell.

Let's take a quick pause.

When we come back in the next section we're going to use kubectl to create a secret tied to our cluster.

So I'll see you in just a minute.