.travis.yaml file = 
```.travis.yaml
sudo: required

services:

	- docker

before_install:

	- curl https://sdk.cloud.google.com | bash > /dev/null;
	
	- source $HOME/google-cloud-sdk/path.bash.inc
	
	- gcloud components update kubectl
	
	- gcloud auth activate-service-account --key-file service-account.json
```

All right, my friends, in this section we're going to start working on our Travis config file.

So without any further delaying on my part, let's flip over to our code editor.

Inside of my root project directory I'm going to make a new Travis dot YAML file.

Now I want you to remember we have a leading period in the name of this file.

So I'm going to put in dot Travis dot YAML like so.

Or we can just do the abbreviation YAML.

Same thing.

So dot Travis dot yml.

So now inside of here we're going to eventually end up with a tremendous amount of configuration.

Some the stuff inside of here is going to look rather familiar.

And a lot of it is going to be pretty new.

So we're going to kind of speed through the stuff that we already understand and get over to some of

the new stuff.

So the first thing I'm going to do is add on sudo required, because we're making use of Docker, I'm

then going to say that we require the Docker service to be pre-installed as soon as we try to run our

build, because we definitely need Travis to build our images and even run that test container.

After that, we're then going to add on a before install flag.

So this is going to be a series of steps that's going to essentially do just about everything up to

that point.

These first four things right here.

Now, the first thing that we're going to do is attempt to download and install the Google Cloud SDK.

So to do so, kind of hard for me to give you a good explanation of where this command is coming from.

Essentially, this is something that you would go and do a little bit of research on and just read up

on how to install the Google Cloud SDK.

Of course, I'm going to show you right now, I just want you to understand that this is kind of something

coming from the outside world.

So we're going to do a curl https colon slash slash SDK cloud google.com.

And then we'll put a pipe.

So that's a vertical symbol right there.

It's shift.

And then the key above your return button.

That is not an L.

So not null not a capital L not a capital I.

It's a pipe symbol.

And we'll say bash greater than sign slash dev null and then a colon like so.

All right.

So this command right here is going to download Google Cloud SDK.

And then everything on the other side of the pipe right here is going to install it locally on our little

instance that is assigned to us by Travis-ci.

After installing it, we're then going to run one other kind of strange command.

I'm going to say source dollar, sign home, Google Cloud SDK and then path dot Bash Inc.

So this right here is going to look at the default install directory of Google Cloud SDK which is by

again by default at home Google Cloud SDK.

And it's going to source the file path Bash Inc.

Essentially, that just means that there is some configuration that's going to modify our shell inside

of Travis-ci inside this file.

And we're going to apply that additional configuration through the source command.

Again these two lines of configuration right here kind of something that you would look up ahead of

time and just be told, hey, run these two commands.

And it's going to set up Google Cloud locally on your machine for you.

Okay.

So then after we install this thing, we're going to make sure that Google Cloud or the CLI is going

to also install the kubectl command, the same one that you and I have been using throughout this course

to manipulate our Kubernetes cluster.

To do so, I'll write out G cloud components update Cube CTL.

Like so.

So that's going to install and update cube CTL inside of our Travis environment.

And again we're going to eventually use this kubectl to apply all those different configuration files.

And some of the other imperative commands we have to run to set our images on each deployment.

Now after doing all this initial setup, we then have to do some authorization with Google Cloud or

this G cloud SDK.

We have to tell it, hey, who's this is who we are.

And here's our password or whatever it might be, everything that it needs to actually log in and get

access to our account.

Now the command for this is going to be a little bit more involved.

We're going to write out the command and then we'll talk about exactly what it's doing.

So I'm going to say gcloud auth activate service account dash dash key file and then service account

dot JSON like so.

All right.

So you might remember back when we were working with AWS in Elastic Beanstalk, we had created that

IAM user and we had assigned it some number of permissions that essentially allowed that kind of user

that we made use of inside of Travis-ci to actually access our copy of Elastic Beanstalk and do a deployment

at some point.

This service account right here, the Active Activate Service account, is essentially equivalent to

that entire IAM system.

So in order to tell Google Cloud or this SDK who we are and give it access to our account, we have

to activate a service account and then provide a set of credentials inside of a file that we are calling

service account dot JSON.

So inside of this file that does not exist yet we haven't created it.

We're going to eventually need to put some information that's going to give clear access to our Google

Cloud account.

Now as you might guess, these credentials that are going to be placed inside that file are extremely

sensitive and in no way, shape or form would we ever want to allow anyone to get access to those credentials.

So let's take a quick pause right now.

When we come back to the next section, we're going to generate this set of credentials that are going

to allow access to be given to our account.

