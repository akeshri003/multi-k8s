
We changed the containerPort from `3000` to `9999`.
![[Screenshot 2026-04-19 at 3.17.34 PM.png]]

and then applied the new changes using - 
`kubectl apply -f client-pod.yaml`

```bash
aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl apply -f client-pod.yaml

The Pod "client-pod" is invalid: spec: Forbidden: pod updates may not change fields other than `spec.containers[*].image`, `spec.initContainers[*].image`, `spec.activeDeadlineSeconds`, `spec.tolerations` (only additions to existing tolerations) or `spec.terminationGracePeriodSeconds` (allow it to be set to 1 if it was previously negative)

  core.PodSpec{

   Volumes:        {{Name: "kube-api-access-gpnt9", VolumeSource: {Projected: &{Sources: {{ServiceAccountToken: &{ExpirationSeconds: 3607, Path: "token"}}, {ConfigMap: &{LocalObjectReference: {Name: "kube-root-ca.crt"}, Items: {{Key: "ca.crt", Path: "ca.crt"}}}}, {DownwardAPI: &{Items: {{Path: "namespace", FieldRef: &{APIVersion: "v1", FieldPath: "metadata.namespace"}}}}}}, DefaultMode: &420}}}},

   InitContainers: nil,

   Containers: []core.Container{

   {

   ... // 3 identical fields

   Args:       nil,

   WorkingDir: "",

   Ports: []core.ContainerPort{

   {

   Name:          "",

   HostPort:      0,

-  ContainerPort: 3000,

+  ContainerPort: 9999,

   Protocol:      "TCP",

   HostIP:        "",

   },

   },

   EnvFrom: nil,

   Env:     nil,

   ... // 14 identical fields

   },

   },

   EphemeralContainers: nil,

   RestartPolicy:       "Always",

   ... // 26 identical fields

  }
```


-: In the last section, we made a change

to our pod configuration file.

That very particular property that we updated

was the image right here.

We then fed that back into kubectl

and we saw the updated pod using the described command.

In this section, I wanna make another change

to this configuration file.

Now, this next change we're going to make

is going to appear to be a very small change,

like similar in nature to changing the image.

But when we try to feed the configuration file

or the updated configuration file in a kubectl,

we're going to very quickly see an error message.

So let's go through this process.

We're gonna see the error message

and we'll talk about why we are seeing the error message

that we're going to see.

All right, so inside my client pod file,

I'm gonna find the container port listed down here.

Now remember, multi-worker doesn't do anything

with container port 3000, no usage whatsoever.

Multi-worker has no ability to receive

or make outgoing requests or anything like that.

So we'll just make an arbitrary change here.

I'm gonna change the container port from 3000

to about 9999, like so.

So totally arbitrary change, no rhyme or reason behind it.

I'm gonna save this file now,

and then we're going to try to update our pod in place

by using the kubectl apply command again.

And again, when we run this command,

you're gonna see an error message and we're gonna talk about

why we are seeing the error message.

All right, So I'm gonna save the file.

I'll then flip back over to my terminal

and I'll do kubectl apply, clientadd.aml again.

Now when I run this command,

we're gonna see this tremendous amount of output right here.

I want you to scroll up to the very top of this message.

And up here, you're gonna see the reason for the error.

Essentially, it says, that we are not allowed

to update any pod configuration piece

other than the image used, some other image property,

some active deadline something, or tolerations property.

So this error message right here is essentially saying

that when we update a pod, we are only allowed

to update these four different configuration properties.

And so, this definitely kind of goes very much

in the face of what I just told you

about how we make updates.

I had just told you two seconds ago that, yeah,

we make updates to existing objects

by updating the configuration file

and then feeding that into kubectl.

But in this case, we just made a very simple change

to our configuration file

and we very quickly saw an error message.

So how do we use

this kind of configuration file based process

of updating something if kubectl won't allow us

to change something as basic as a container port?

Well, of course, there is a workaround to this.