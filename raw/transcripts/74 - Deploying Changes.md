-: In the last section,

we put together our certificate.YAML file.

Now at the end of this section

I said there was one last piece

of configuration we have to do.

It was a little bit off base there.

We do have to do one last piece of configuration

to our ingress, define inside

of our ingress service.YAML file.

But we actually only do that last piece of configuration

after we have successfully retrieved our certificate.

So the general flow here is that we get our issuer,

we get our certificate, we deploy that.

Cert manager is going to go through this

back and forth process and get our certificate.

And then only once we have it and we have it enhanced

stored inside of a secret inside of our cluster

are we going to go back to our ingress service

and tell it to update itself to use this new certificate.

So in other words, we're now at the point

where we need to redeploy our application

so that the issuer and the certificate files

are created as objects inside of our cluster.

Once we do that deployment, the cert manager

that we already installed using helm is going

to see the issuer and certificate

and automatically go to this process

of reaching out to let's encrypt.

So to deploy our application,

I'm going to flip back over to my terminal.

Now I've been making all these changes on the devel branch.

To be honest, I don't really wanna have to go

through the process of creating a pole request

and all that stuff again and have to wait

for multiple runs of our travis CI server.

So instead, I'm just going to change

back over to my master branch

and commit all of the work over on that branch

and then push it directly to Master on GetHub.

So to get started, I'm going to do a get checkout master.

I'll then verify that I see all the changes

that I just made to my project.

So there's the two files we just added.

I'm gonna do a get ad and a commit

so I'll say added,

certificate and issuer.

And then I'm going to do a get push origin master.

Oops, it looks like I've made a couple changes

to my project up on GetHub.

My mistake.

So I need to just pull my changes down to do.

So I'll do a get all origin master,

and then it's going

to automatically create a merge request here.

So I'll do a right quit like so.

So now I can do a get push origin master again.

And there we go.

All good to go.

All right, so as usual

this is going to be picked up by travis CI.

It's gonna run our tests and then it's going to attempt

to apply all the different config files

in the K eight directory.

Since we just created certificate.YAML and issuer.YMAL,

they will both be created automatically

by our Cube CTL command running over

on the Travis CI instance.

That will then update our cluster,

creating those two objects.

And then we should see the cluster

essentially start to spring to life

with the two new resources

either inside of our workload section right here

or the configuration section over here.

And honestly, we should actually eventually

see the configuration section,

get that new certificate

that we just asked to be created and stored.

All right, so we're gonna take a pause right here.

I'm gonna let Travis CI kick in and do its little thing.

And now I'm just gonna stick around

for about 5 or 10 minutes and allow time for Cert Manager

to reach out and get that certificate.

So in total, I'm gonna wait maybe 10, 15 minutes or so.