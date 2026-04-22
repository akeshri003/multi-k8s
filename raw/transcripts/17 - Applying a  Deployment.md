![[Screenshot 2026-04-19 at 3.44.56 PM.png]]

This is like an imperative language update to our cluster. To delete an existing resource inside our cluster, its hard to use a declarative method to delete an object from within our cluster.

```
aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl get pods

NAME         READY   STATUS    RESTARTS      AGE

client-pod   1/1     Running   1 (41m ago)   40h

aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl delete -f client-pod.yaml

pod "client-pod" deleted from default namespace

aryankeshri@Aryans-MacBook-Air-2 simplek8s %
```

-: In the last section,

we finished up our client deployment configuration file,

and we're now ready to toss this thing off to kubectl,

and see it running on our local cluster.

Now, before we do, I wanna give you a very quick reminder.

If you flip over to your terminal,

and do a kubectl get pods,

you'll recall that we still have

the original pod that we had created

with our client-pod.yaml configuration file.

I think that would be really nice if we got rid of this pod

or somehow deleted it before we sent out our deployment.

That way when we get our pods in the future,

we will only see the one pod

that was created by the deployment

as opposed to the one pod from the deployment

and this one pod from the original

client pod configuration file we had created.

So I wanna figure out how we can delete this existing pod.

Now to remove an existing object,

we're going to be making use of the configuration file

that was used to create it.

So in order to remove an existing object,

we're going to run kubectl delete -f,

and it will pass in the config file

that was used to create that object.

When we pass in that configuration file,

kubectl is going to look at the file.

So here's the client pod file,

it's gonna look at this file,

it's going to look at the kind property right here,

and the name specified as well.

Kubectl is then going to try to find an object

with the same type that has the same name.

And when it finds it, it's going to delete it.

Now, one thing I want to mention very quickly here

is that this delete command seems an awful lot

like an imperative update to our cluster.

And in fact, it really is.

This is an imperative update.

We are issuing a very direct command that says,

"Hey Kubernetes,

I want you to make this very discrete change

to the state of our cluster."

Now unfortunately,

this is something that we really just can't quite get over.

When we want to delete, create a resource,

or change an existing resource,

we pass in an updated configuration file.

And so we're kind of leaving it up to Kubernetes

to make the appropriate changes.

But when we want to delete our resource, unfortunately,

if we wanted to delete some object inside of our cluster,

we would have to kind of take the entire state

of our entire cluster,

and then like subtract out that one object.

In other words,

it's just kind of hard to picture a way

to make a declarative update

that will delete an existing object.

And so this is going to be the one kind of location

where we kind of fall back to the idea

of an imperative update to the state of our cluster.

Okay, let's give this a shot.

So back at my command line,

I'll do kubectl delete -f,

and then I'll specify the client-pod.yaml file.

And then we'll see that the pod

with a name of client pod was deleted.

Now if it appears that this command just seems to hang,

it will eventually resolve after about 10 seconds or so.

When the pod gets deleted,

it's going through the same process in the background

that usually gets applied

to deleting a container with the Docker CLI.

Remember, when you delete a container,

or stop a container I should say,

the container is given 10 seconds to resolve,

and then eventually it just gets killed.

That's what happens in the Docker world.

And the same thing is what happens

when we delete a running pod.

It gets 10 seconds to resolve

and eventually turn itself off,

and then after those 10 seconds,

the thing automatically just gets axed entirely.

If we now do a kubectl get pods,

we'll see that we no longer have any running pods.

Okay, so now that we've cleaned things up a little bit.

Let's try applying our configuration file

of client-deployment.yaml to our cluster.

After we apply this thing,

we should see a new single running pod

using the multi-client image.

So I'm gonna flip over to my terminal,

and I'll do a kubectl apply -f client-deployment.yaml.

And then we very quickly see

that the deployment was created.

So we can now print out the status

of all of our pods with kubectl get pods, and we'll see

that there is exactly one pod running right now,

and it has a randomly generated name

very clearly tied to the client deployment

that we just created.

We can also print out the status of that deployment itself

using the command kubectl get deployments.

So we're still using the get command,

the same one we were using before,

but we're changing the type of object

that we are looking for.

So now as opposed to looking for pods,

we're looking for deployments.

So I'll run that,

and we'll see that we have one deployment

called client deployment.

You'll also notice a couple of interesting columns on here.

We have ones across the board

for desired, current, up to date, and available.

Desired is a reference to the number of replicas

or the number of pods that this deployment

wants to eventually have.

So at present inside of our configuration file,

we said that we want exactly one replica running,

and so we have desired of one.

We then see current, so that's the number,

of pods that are up and running.

We have up to date, which is also one.

So at any point in time

that you make a configuration change to your deployment,

specifically a configuration change

to the template down here,

the deployment would automatically

mark all the existing pods as being out of date.

And so we might see up to date go down to zero.

And then as the existing pods get updated or recreated

with that new configuration state,

they'll eventually restore this up to date field.

And then finally available right here

is the number of pods controlled by this deployment

that are ready and available

to accept incoming client traffic,

or essentially just successfully running their containers

with the appropriate configuration for each one.

Okay, so that's it.

So we've now seen how we can use a deployment

to create a set of pods,

or in this case just one single pod.

Now the last thing I wanna do is still make sure

that we are able to visit our running application

inside of the browser.

So we want to make sure that this pod right here

is in fact successfully running the multi-client image,

and I wanna make sure

that we can still access our React project files.

So let's take a quick pause right here.

we'll come back to the next section,

And we'll make sure that we can still access this pod

or this container in our browser in the next section.