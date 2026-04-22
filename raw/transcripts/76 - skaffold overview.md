![[Pasted image 20260423010827.png]]
  
![Lecture thumbnail](https://mp4-cdn77.udemycdn.com/2019-06-27_19-17-33-1082d4793b5ffed3895b386016e80350/2/thumb-sprites.jpg?secure=EyTKy0dZ_LSkF9NnIaf7mw%3D%3D%2C1776902712)

3:48 / 3:54

Up next

## 

304. Installing Skaffold

-: All right my friends, we've now got our application

up and running with Kubernetes

but there's one last thing I want to show you.

Right now, we don't really have a good setup

for running our code locally

and doing local development with Kubernetes.

Sure, we can deploy to our mini cube cluster

but going through the process of actually

developing our application is a little bit awkward.

Let me show you what I mean.

If you recall back on the Docker Compose stuff we did,

we had set up a client container for our React project.

When we made use of Docker Compose, we set up a volume

that mapped our local React Project directory

into that volume.

So essentially, we were sharing our source code

between our local file system and that client container.

That meant that anytime we changed our code

on our local machine, it updated the code inside

the container as well, and the React application

automatically updated right before our eyes.

Now this system only works with Docker Composed right now

we don't really have anything equivalent for Kubernetes.

So right now if you and I wanted to make some change

to our local React Project during development,

we don't have any easy way to somehow inject

that source code into our client pod.

Instead, we would have to completely rebuild

the client image and then rerun that Cube CTL apply command.

That's definitely a pain.

So let me show you a better way to handle

this development process.

We're going to be making use of a tool called Scaffold.

This is a command line tool separate from Kubernetes

but designed to be used with Kubernetes

just to facilitate local development.

Here's how Scaffold works.

Scaffold is going to watch our local React Project

directory for changes.

So you and I might then open up our code editor

and change some React component or whatever else.

Once we save that file, Scaffold is gonna see that

a change occurred and then Scaffold

is gonna jump into action.

Scaffold is all about somehow taking that change in our code

and getting it reflected inside of our Kubernetes cluster.

It can do that with one of two different modes

and we're going to explore both these different modes.

The first way that Scaffold is going to somehow

update our client pod inside of our cluster

is just automatically rebuild

the entire client image from scratch.

When I say from scratch, I don't mean like rerun

every single step including installing dependencies,

it will be just a normal Docker build process.

Scaffold is going to tell Docker to rebuild the client.

Docker is going to see that the only change we made

was to our source code, and so it's essentially

just going to stick that source code

into a new updated image.

Scaffold will then take that image and update

our local Kubernetes cluster, and then we should see

our updated application appear.

Now that process is still a little bit slow

because we have to go through that entire sequence

of rebuilding the image, even though it's kind of just

the last step we are changing and then we still have to go

through that redeployment process.

So the other way that Scaffold can work is mode two

which is to essentially take the updated files

or the changes we made in our local React Project directory

and just kind of magically inject them into our client pod.

It will then be up to our client pod

to somehow automatically update itself.

So with mode two right here

if we want to go through mode two

we need to make sure that our client pod is running

in such a mode where it's going to see these updated files

and automatically update itself.

We've already set up Create React App.

Remember, Create React App is going to automatically

refresh the change, or excuse me

refresh the page anytime we change a file.

We also set up our Node project as well

the backend API server with Nodemon.

So Nodemon watches for changes

inside of our project directory.

So right now our API server and the client pod

will automatically refresh given this updated source code.

So we currently have a project that's kind of well-suited

for mode number two.

Okay, so now we've got an overview of what Scaffold does.