
-: In the last couple of videos

we put together a set of three different config files.

So one for the worker and two for our server.

We're now gonna flip back over to our command line

and use kubectl apply to take those three new config files

and apply them to our cluster.

We're just going to make sure

that there's no distinct error messages

or that we didn't make any big typos or anything like that.

Now when we launch these different deployments,

it's entirely possible they're just going

to crash and burn because we have not yet set up

any environment variables to tell the worker

or the server how to connect to Redis or Postgres,

because we haven't even set those things up yet.

So if the deployments or the pods

that they are running just crash and burn horribly,

that's totally fine.

What we're doing right now is just checking

to see if we have any typos or anything like that.

So I'm gonna flip over to my terminal.

I'm gonna make sure that I'm inside of my complex directory,

and then I'm going to apply

the entire k eights directory again.

So remember, we do not need to pass in one file at a time.

We can pass in a whole directory.

When we apply that directory,

we're also going to be reapplying the two client files

that we had already applied previously,

but that's totally okay,

'cause remember when Kubernetes gets a config file,

it's gonna look at the name of the object

we're trying to create and its type.

And if a object inside of our cluster already exists

with the same name and the type,

an additional object will not be created.

Kubernetes is just going to try

to update that existing object

if there's any difference inside of the config file.

So what I mean to say by all this is that

whenever we make a change to our config file,

or any time that we create a new one

or whatever it might be,

we can pretty safely just take the entire K eights directory

and apply everything in there and generally be okay.

Now there definitely are some side effects to that later on

when we start adding in a couple more advanced objects

that we're gonna do much later in the course.

But at least for right now, when we're only working

with deployments and services, kind of free form,

you can just throw everything

inside of your config directory into kubectl,

and you're going to be okay.

All right, so enough talking.

We're gonna do kubectl apply F K eights, like so.

Now you'll notice that Kubernetes very correctly noticed

that there are already two objects inside the cluster

with the deploy type and name

of both these services right here, where both those objects

and the config associated with both them

has not been changed by any stretch of the imagination.

So those are totally unchanged.

However, the three other objects here are being created.

Let's now do a kubectl get pods,

and so we will see a list of all the different pods

that have been created.

It looks like we have the server

and the worker running here successfully.

So even though I kind of thought

that they were gonna crash and burn,

it looks like they're doing okay.

We can do a get deployments,

and see that we now have a total of three deployments.

And then finally, we can do a get services, and see

that we have both the original client cluster IP service

and the new server cluster IP service.

Now as a quick reminder,

we could very quickly use the kubectl logs command

to try to pull some logging information

out of these new pods, and kind of get a better sense

of whether or not they are working correctly.

We could also choose to reconfigure our docker client

to directly access the containers

that are running the multi-server

and the multi-worker images and pull logs that way.

Solely up to you.

But just because it's a little bit easier,

I'm going to pull logs using the kubectl CLI.

So I'll first print out my list of pods again,

and then I'll just take the name

of one of these random deploy, or excuse me,

yeah, one of the random pods right here.

I saw the word deployment

and confused myself for a second, but we're good.

So I'm gonna take the name right there.

I'm gonna copy it.

And then I'll do a kubectl logs, and then the name like so.

All right, so we can see that in fact, no JS was executed,

and that is our default MPM start command.

That's what we expect to see.

But then very shortly after that,

we saw a refuse to connect right here.

Now the refuse to connect is coming directly

from us trying to connect to Redis,

but no copy of Redis is available.

You'll notice that it did default to using port 5432.

It only defaulted to that because the Redis library

that we are using inside of that image defaults

to use port 5432.

So even though we didn't specify a port yet

for this server to connect to, it just defaulted to 5432.

And of course, when we eventually add in

an environment variable to specify the port,

even though it has a default,

we're still going to make sure that we very directly

and distinctly say it's supposed to be port 5432.

All right, so it looks like everything is running

as well as can be expected.

So we're gonna take another quick break right here.

When we come back to the next section,

we'll put together another set of config files for Redis,

and then we'll eventually move on to Postgres as well.

So quick pause, and I'll see you in just a minute.

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl apply -f k8s

service/client-cluster-ip-service unchanged

deployment.apps/client-deployment unchanged

service/server-cluster-ip-service created

deployment.apps/server-deployment created

deployment.apps/worker-deployment created

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl get pods

NAME                                 READY   STATUS    RESTARTS   AGE

client-deployment-576bc76988-d7brf   1/1     Running   0          34m

client-deployment-576bc76988-mm5gb   1/1     Running   0          34m

client-deployment-576bc76988-nch6x   1/1     Running   0          34m

server-deployment-56685c959c-99zpc   1/1     Running   0          81s

server-deployment-56685c959c-b8k7c   1/1     Running   0          81s

server-deployment-56685c959c-nxjd6   1/1     Running   0          81s

worker-deployment-7c49d8f5f4-wzd9f   1/1     Running   0          81s

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl get deployments

NAME                READY   UP-TO-DATE   AVAILABLE   AGE

client-deployment   3/3     3            3           34m

server-deployment   3/3     3            3           92s

worker-deployment   1/1     1            1           92s

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl get services

NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE

client-cluster-ip-service   ClusterIP   10.106.232.84   <none>        3000/TCP   35m

kubernetes                  ClusterIP   10.96.0.1       <none>        443/TCP    29d

server-cluster-ip-service   ClusterIP   10.97.132.165   <none>        5000/TCP   100s

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl logs server-deployment-56685c959c-99zpc

  

> @ start /app

> node index.js

  

Listening

{ Error: connect ECONNREFUSED 127.0.0.1:5432

    at TCPConnectWrap.afterConnect [as oncomplete] (net.js:1161:14)

  errno: 'ECONNREFUSED',

  code: 'ECONNREFUSED',

  syscall: 'connect',

  address: '127.0.0.1',

  port: 5432 }