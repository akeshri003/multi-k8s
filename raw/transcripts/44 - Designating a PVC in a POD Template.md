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

			volumes:

				- name: postgres-storage

				  PersistentVolumeClaim:

					claimName: database-persistent-volume-claim

			containers:

				- name: postgres

				  image: postgres

				  ports:

					- containerPort: 5432

				  volumeMounts:

					- name: postgres-storage

					  mountPath: /var/lib/postgresql/data

					  subPath: postgres
```

In the last section, we finished up our persistent volume claim.

I'm now going to flip over to my Postgres deployment file.

So I'm going to find the template section of this.

Remember this is the template that is used for every pod that is created by this deployment.

And there's only ever going to be one pod at a time.

We are going to update this template section and tell this pod that when it is created, it needs to

request some type of long term storage.

In other words, a persistent volume that meets all the requirements that were advertised by this persistent

volume claim that we just put together.

So inside of my Postgres deployment file, I'll find the template section.

And then the spec inside there.

And inside the spec we're going to add on a new key value pair.

So I'm on the same indentation level as containers right here.

And I'm going to say volumes.

This is going to be an array.

So I'll put a dash in and then I'll say name of Postgres storage.

And then persistent.

Persistent volume claim.

Claim name.

And this is going to be the name of the claim that we had just put together in the other file.

It's name was database persistent volume claim.

So the claim name will be database persistent volume claim.

Okay.

So this right here is what sets up the request on the pod to reach out to Kubernetes and say I need

some type of long term storage that meets all the requirements that are laid out inside of this database

persistent volume claim object.

And that's what we just put together over here.

So this line alone is what's going to make Kubernetes realize that it needs to go over to either the

local hard drive.

If we are in the case of your local environment or some cloud provider, in the case of being deployed

in production and say, hey, I need to somehow source or somehow get some slice of storage that has

this access mode and storage of two gigabytes.

So all this is going to do right here is allocate that storage.

Once we allocate that storage, once we get it available, we need to actually assign it for use by

all the different containers that are in use by our pod.

So in addition to this volume section right here, we're also going to add on some config to our container

section as well.

So inside my container section I'm going to add in a new line here.

And I'm going to get on the same indentation level as name image and ports.

And I'll say volume mounts.

So this is going to say hey all right we just got access to the storage.

And here's how I want it to be used inside of my container.

So I'm going to add in a dash here because this is an array.

We can have multiple volume mounts on a single container.

I'll give it a name of Postgres Postgres storage.

Now this right here is the most important part.

Notice how the volume name and the volume mount name are identical.

So when you put the name right here, it means go back out to the volumes entry Century and find some

piece of storage that we just asked Kubernetes for.

In this case, it's going to find this piece of storage right here.

And that piece of storage is going to be used for this particular volume mount.

We're then going to put in a mount path.

The mount path is designating where inside the container this storage should be made available.

So in other words we're going to put in a little folder reference right here.

And then anything that the container stores at that folder or inside that directory will be actually

stored inside of our volume.

Remember this is at the end of the day, pretty darn similar to the Docker volumes that we had used

previously.

So for the mount path, we're going to designate the data directory that Postgres uses for storing data

on the hard drive, because that's the actual data that we want to back up.

We want to back up all the data that Postgres is storing on the hard drive.

The default storage location for Postgres is var lib PostgreSQL slash data like so.

Okay, now for a normal volume that would be it.

So if this was just a normal application where we're just trying to set up some persistent storage,

that's really all we have to do with Postgres.

Postgres in particular.

We're going to put in one additional little option here.

So as an additional option I'm going to also put in sub path is Postgres like.

So the sub path option means that any data inside the container that is stored inside of mount path

is going to be stored inside of a folder called Postgres, inside of the actual persistent volume claim.

So if we ran our application for some amount of time and then save some data to our Postgres database

and then eventually opened up our persistent volume, we would see that all the data that was saved

to this folder is nested inside of a folder called Postgres inside the persistent volume.

Now, like I said, this is something very specific for Postgres.

It's just because if you try to start up Postgres by default with something that it thinks is a volume

mount, it's going to say, hey, I don't want to save data here.

And so by having Postgres instead save data into a sub folder inside there.

It's going to override that default behavior.

Now saving data into a volume with Postgres is totally fine.

Totally fine.

It's just that Postgres in some cases thinks that you're not necessarily working with Docker.

It gets.

It's a long story.

Let's just leave it at that.

This is just some very particular stuff around Postgres.

Probably not super interesting to you or what you're going to be working on in your own application.

So I'm just going to stop right there.

All right.

So that's pretty much it.

Let's do a very quick review here.

We put together the persistent volume claim that tells Kubernetes that we want to find a storage option

with these requirements.

When we put together our Pod template we said we want to have a volume available that matches these

requirements.

And then inside the container we put together some actual options to say take that volume and make it

available inside of this very particular container.

So that's the entire story.

Let's take a quick pause right here.

When we come back to the next section, we're going to try to apply all these updated config files and

just make sure that everything is working the way we expect.