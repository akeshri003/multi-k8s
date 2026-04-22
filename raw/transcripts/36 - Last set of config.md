```postgres-deployment.yaml
apiVersion: apps/v1

kind: Deployment

metadata:

	name: postgres-deployment

spec:

	replicas: 1

	selector:

		matchingLabels:

			component: postgres

template:

	metadata:

		labels:

			component: postgres

	spec:

		containers:

			- name: postgres

			  image: postgres

			  ports:

				- containerPort: 5432
```

```postgres-cluster-ip-service.yaml
apiVersion: v1

kind: Service

metadata:

	name: postgres-cluster-ip-Service

spec:

	type: ClusterIP

	selector:

		component: postgres

	ports:

		- port: 5432

		  targetPort: 5432
```

Presenter: In this section, we're going

to get through our last set of boring config

for the Postgres deployment

and its associated cluster IP service.

So I'm sure you don't need any clever

or witty comments from me at this point.

I'm sure you just wanna get this stuff done.

So let's get to it.

I'm gonna flip on over to my code editor

inside of my K8s directory.

We'll do our Postgres deployment .yaml file.

Now again this is gonna be a file

that's very much identical

to the Redis stuff that we just put together

and even the other different objects

we had put together before that.

I still do not recommend that you ever do a copy paste

of these config files just because it's

so incredibly easy to forget to update

some specific property.

Far safer to just rewrite it.

But that's just my personal opinion.

All right, so we'll do our API version

of apps V1 for our deployment.

This is going to be an object with a type of deployment.

We'll set up our metadata if I can spell it correctly

with a name of Postgres deployment, and then we'll set up

our spec that's going to configure the deployment itself.

So I again want to only have one replica running

of my Postgres container inside of its given pod.

Just very much like our Redis deployment

that we just put together.

We technically can create Postgres

in a sort of cluster of different instances

that are going to all work together

to increase the availability and bandwidth of our database.

However, again, that's something

that's kind of outside the realm

of Kubernetes that we're talking about right now.

So we're just gonna focus

on running a single replica as it stands.

After that, we'll put down our selector.

We're gonna say look for a label

that is matching component Postgres.

We'll then set up our pod template.

So we'll give it a metadata property

with labels of component Postgres.

And then again, I'm going to make sure

that I un-indent back to be on the same

indentation level as metadata.

We'll define our spec for all the different pods

that get created by this deployment.

I'll say here's our list of containers.

I want it to have a name of Postgres.

I want to use the image Postgres.

And then finally, we're going to set up

the ports here as well.

So the default port with Postgres, which we are using

we're not going to reconfigure or anything like that

will set up a container port of 5 4 3 2.

Again, default port for Postgres.

All right, so that looks good.

Now remember, very similarly to all the Redis

and Express API stuff that we had done previously

to get our express and our worker pods

up and working correctly

or more specifically the containers inside there,

they need to have that set of environment variables

that are going to make sure

that they can successfully connect to this copy of Postgres.

So we do have to set up a little bit

of environment variables,

not only inside of this file

but inside of the worker and server deployments as well.

We're gonna take care of that in just second.

But for right now, I wanna first put together

the cluster IP service, and then we'll come back

and talk about all these environment variables

and this PVC thing and all that stuff.

Essentially, I just wanted to get the boring parts

out of the way here at the very start

of these repetitive config files.

And then go over to the more interesting stuff

like the environment variables

and the Postgres persistent volume claim.

All right, so that's our deployment.

We're going to also make inside of our K8s directory

the Postgres cluster IP service .yaml file.

And inside of here, completely identical

to the Postgres one we just,

or excuse me, the Redis one we just put together

I'll do an API version of v1.

We are making a service.

It'll have a metadata name of Postgres cluster IP service

and we'll give it a spec with the type of cluster IP.

And then finally, to make sure that we're,

or not finally, we have two other properties

to do here, but one of the two is going to be

this selector where we'll tell this thing

what set of pods it's going to look for

and as usual, we'll give it component along with the name

or the label that we had applied to it, which was Postgres

as we can verify back inside of our template section

over here inside the deployment.

All right, so now the real last step,

for real this time around,

we're gonna set up our ports.

We're gonna say the default port

of 5 4 3 2 to connect to Redis.

And we're not gonna do any type

of remapping this time around as well.

Up target port. There we go.

All right, so that looks like it's just about it.

Now let's take these two new configuration files

and we're going to apply them to Cube CTL as well.

And then we'll check out our different sets of pods

and make sure that everything is running successfully

before we start to come back

and take care of the Postgres persistent volume claim thing

and some environment variables as well.

So at my terminal, still inside of my complex directory

we'll do our Cube CTL apply, K8s.

Now we should see that we have the Postgres IP service

and the Postgres deployment as well.

So just as before, we'll do our get pods.

And now one of these names is really long

so it wants to kind of wrap the levels with me.

So I can see my Postgres deployment right here.

The container is being created.

I'll do get pods again, and I can now see that it is

in fact up and running for the Postgres deployment.

And I'll make sure that I do my get services as well

and somewhere inside of here, Postgres cluster IP service.

Very good.

And I got pop port 5 4 3 2.

All right, so I think that we are pretty much all set.

So that's just about it on our very basic,

very simplistic config files

for all the different deployments and associated services.

So let's take a pause right here.

When we come back to the next section, we're gonna talk

about some additional work that we need to do

to make sure that the server can connect

to Redis and Postgres, and to make sure

that the worker can connect to Redis as well.

And also let's not forget this

little PVC thing down here.

Okay so, quick pause and I'll see you in just a minute.