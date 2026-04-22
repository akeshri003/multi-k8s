-: In the last section, we spoke about the big differences

between a persistent volume claim and a persistent volume.

Remember, a volume claim is an advertisement.

It is saying, hey, all the pods inside this cluster

you can choose from a 500 gigabyte option

or a one terabyte option.

When we write our our pod config,

we are gonna say, hey this pod

needs the 500 gigabyte option.

And so you can kind of imagine

that our pod config is going to be handed off

to Kubernetes with this kind of option tied to it.

Kubernetes is gonna see that option

and it's gonna say, oh, okay, I get it.

I need to find an actual instance of storage

inside of my cluster that meets these requirements.

It has to have 500 gigabytes worth of storage.

If Kubernetes does not have a instance of storage

like that ready to go,

then it's going to try to create one on the fly

and then attach it to the pod that gets created.

Now, I can give you diagrams

about this stuff all day and verbal descriptions all day

but to really understand the stuff

you really have to put together a config file on your own.

So in this section

we're going to write out a persistent volume claim.

And remember, this is going to essentially

advertise a possible storage option

that can be attached to a pod config.

So inside of my code editor,

I'm gonna find my K eights directory.

And inside of here, I'm going to make a new file

called database persistent volume claim.yaml.

Now I type that out rather quickly.

Remember that you can always see my file name

at the top here inside this tab.

All right, so inside of here we're gonna write

out a pretty good amount of config.

Some of it is gonna look very familiar

and some of it is going to be a little bit new.

So we'll do the familiar stuff first.

I'll say API version is v1.

Our kind will be persistent volume claim.

We'll do a metadata that has a name of database

persistent volume claim, and then we'll add a spec.

So the spec is where some interesting things

start to come into play.

I think you'll agree with me

that everything up here looks pretty darn familiar.

So for our spec, we're going to give an access modes.

Notice how there is an S on there.

This is going to be an array,

so I'm gonna put a dash in like so,

and then I'm gonna give this a single value.

I'll say read, write once.

On the next line, I'm going to unindent

so that I'm on the same indentation level as access modes.

I'll do resources.

Requests, storage is to GI like, so.

Okay. So that's it.

That is all the config that we need

for our persistent volume claim.

Let's take a quick pause right here.

When we come back to the next section,

we'll talk about exactly

what is going on inside the spec section.

```database-persistent-volume-claim.yaml
apiVersion: v1

kind: PersistentVolumeClaim

metadata:

	name: database-persistent-volume-claim

spec:

	accessModes:

		- ReadWriteOnce

	resources:

		requests:

			storage: 2Gi
```