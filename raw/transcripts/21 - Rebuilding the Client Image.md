
-: In the last section,

we started to investigate

how we can get a deployment to make sure

that all of our pods are using the latest version

of an image that is available.

So in the last section,

we flipped our deployment

to use the multi-client again,

we're now going to go back to the complex project,

that's the multi-container project

that we worked on previously,

and we're going to update the multi-client image.

So inside of my terminal,

I'm gonna open up a second tab,

and then I'll navigate over

to the complex project

that we had previously put together.

Remember that inside of here

we have that client folder,

that's where the multi-client project is coming from.

So I'm gonna change into that directory,

and then I'm gonna start up my code editor

inside of that client folder.

All right.

So here we go.

So I've got my client project right here,

inside of the client project is that src folder,

and inside there are a couple

of different react components.

And so to update our multi-client image

and get kind of a change this thing

that we can very easily see inside of our browser

when we run this application,

I'm gonna open up the app.js file.

And inside of here,

I'm gonna find the render method,

inside there we have an h1 right here.

I think that a very easy to detect change

that we could make would be

to alter the title that appears on a page.

So rather than simply saying Fib calculator,

I'm gonna put on Fib calculator version two, like so.

So that's just gonna be a very simple change

that will appear inside of our browser,

and it'll make it very obvious

when we are using a newer version

of this particular image.

All right.

So I'm gonna save this file,

and then we're going to close that code editor

and we'll then rebuild our image

and push it back up to Docker Hub.

So back at my command line,

I'm gonna make sure that I'm still inside

of that client directory.

We'll do a Docker build,

and we'll be sure to tag this thing

with your Docker ID/multi-client, like so.

And then I'll specify my build context

with a dot at the very end.

Don't forget the dot.

Okay, so I'm gonna run that,

it's gonna rebuild the entire image.

Now remember, we have to rerun

the build portion of that script

because we made a change

to our source code.

After all that,

we finally get our new image created.

So now the last thing we have to do

is push this new image

up to Docker Hub.

So I'll do a Docker push my Docker ID/multi-client,

like so.

Okay?

So it's gonna push that set of changes up.

And there we go,

that's pretty much it.

So now Docker Hub has the latest version

of our multi-client image.

And now it's just up to us to figure out

how we can somehow get our deployment

to make use of that newer image

for all the pods that it manages.

So, let's take a quick pause right here.

We'll come back to the next section,

and we're gonna figure out

how we can somehow get our deployment

to update all of the pods that it manages.