
![[Screenshot 2026-04-19 at 3.04.42 PM.png]]![[Screenshot 2026-04-19 at 3.05.02 PM.png]]

```bash
aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl get pods

NAME         READY   STATUS    RESTARTS   AGE

client-pod   1/1     Running   0          2m30s

aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl get services

NAME               TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE

client-node-port   NodePort    10.102.169.232   <none>        3050:31515/TCP   3m8s

kubernetes         ClusterIP   10.96.0.1        <none>        443/TCP          27d

aryankeshri@Aryans-MacBook-Air-2 simplek8s % docker ps

CONTAINER ID   IMAGE                             COMMAND                  CREATED          STATUS          PORTS                    NAMES

75687d3b61e0   stephengrider/multi-client        "nginx -g 'daemon of…"   26 minutes ago   Up 26 minutes                            k8s_client_client-pod_default_2a7d0d41-b9cd-4492-afc2-06189fefb577_0

e9809d46cc26   redis/redis-stack-server:latest   "/entrypoint.sh"         3 weeks ago      Up 11 days      0.0.0.0:6379->6379/tcp   redis-stack

aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl apply -f client-pod.yaml      

pod/client-pod configured

aryankeshri@Aryans-MacBook-Air-2 simplek8s % 

aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl get pods

NAME         READY   STATUS    RESTARTS      AGE

client-pod   1/1     Running   1 (37s ago)   40h

aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl describe pod

Name:             client-pod

Namespace:        default

Priority:         0

Service Account:  default

Node:             docker-desktop/192.168.65.4

Start Time:       Fri, 17 Apr 2026 22:51:10 +0530

Labels:           component=web

Annotations:      <none>

Status:           Running

IP:               10.1.0.14

IPs:

  IP:  10.1.0.14

Containers:

  client:

    Container ID:   docker://e45d42b77c18d3a53e053b18bccdfc7a0891bc316fb092305f939cf093ebcf19

    Image:          stephengrider/multi-worker

    Image ID:       docker-pullable://stephengrider/multi-worker@sha256:5fbab5f86e6a4d499926349a5f0ec032c42e7f7450acc98b053791df26dc4d2b

    Port:           3000/TCP

    Host Port:      0/TCP

    State:          Running

      Started:      Sun, 19 Apr 2026 15:03:37 +0530

    Last State:     Terminated

      Reason:       Completed

      Exit Code:    0

      Started:      Fri, 17 Apr 2026 22:52:05 +0530

      Finished:     Sun, 19 Apr 2026 15:03:02 +0530

    Ready:          True

    Restart Count:  1

    Environment:    <none>

    Mounts:

      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-gpnt9 (ro)

Conditions:

  Type              Status

  Initialized       True 

  Ready             True 

  ContainersReady   True 

  PodScheduled      True 

Volumes:

  kube-api-access-gpnt9:

    Type:                    Projected (a volume that contains injected data from multiple sources)

    TokenExpirationSeconds:  3607

    ConfigMapName:           kube-root-ca.crt

    Optional:                false

    DownwardAPI:             true

QoS Class:                   BestEffort

Node-Selectors:              <none>

Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s

                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s

Events:

  Type    Reason   Age                From     Message

  ----    ------   ----               ----     -------

  Normal  Killing  2m14s              kubelet  spec.containers{client}: Container client definition changed, will be restarted

  Normal  Pulling  2m13s              kubelet  spec.containers{client}: Pulling image "stephengrider/multi-worker"

  Normal  Pulled   99s                kubelet  spec.containers{client}: Successfully pulled image "stephengrider/multi-worker" in 34.799803308s

  Normal  Created  98s (x2 over 40h)  kubelet  spec.containers{client}: Created container client

  Normal  Started  98s (x2 over 40h)  kubelet  spec.containers{client}: Started container client

aryankeshri@Aryans-MacBook-Air-2 simplek8s %
```

```bash
aryankeshri@Aryans-MacBook-Air-2 simplek8s % kubectl describe pod client-pod

Name:             client-pod

Namespace:        default

Priority:         0

Service Account:  default

Node:             docker-desktop/192.168.65.4

Start Time:       Fri, 17 Apr 2026 22:51:10 +0530

Labels:           component=web

Annotations:      <none>

Status:           Running

IP:               10.1.0.14

IPs:

  IP:  10.1.0.14

Containers:

  client:

    Container ID:   docker://e45d42b77c18d3a53e053b18bccdfc7a0891bc316fb092305f939cf093ebcf19

    Image:          stephengrider/multi-worker

    Image ID:       docker-pullable://stephengrider/multi-worker@sha256:5fbab5f86e6a4d499926349a5f0ec032c42e7f7450acc98b053791df26dc4d2b

    Port:           3000/TCP

    Host Port:      0/TCP

    State:          Running

      Started:      Sun, 19 Apr 2026 15:03:37 +0530

    Last State:     Terminated

      Reason:       Completed

      Exit Code:    0

      Started:      Fri, 17 Apr 2026 22:52:05 +0530

      Finished:     Sun, 19 Apr 2026 15:03:02 +0530

    Ready:          True

    Restart Count:  1

    Environment:    <none>

    Mounts:

      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-gpnt9 (ro)

Conditions:

  Type              Status

  Initialized       True 

  Ready             True 

  ContainersReady   True 

  PodScheduled      True 

Volumes:

  kube-api-access-gpnt9:

    Type:                    Projected (a volume that contains injected data from multiple sources)

    TokenExpirationSeconds:  3607

    ConfigMapName:           kube-root-ca.crt

    Optional:                false

    DownwardAPI:             true

QoS Class:                   BestEffort

Node-Selectors:              <none>

Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s

                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s

Events:

  Type    Reason   Age                  From     Message

  ----    ------   ----                 ----     -------

  Normal  Killing  4m15s                kubelet  spec.containers{client}: Container client definition changed, will be restarted

  Normal  Pulling  4m14s                kubelet  spec.containers{client}: Pulling image "stephengrider/multi-worker"

  Normal  Pulled   3m40s                kubelet  spec.containers{client}: Successfully pulled image "stephengrider/multi-worker" in 34.799803308s

  Normal  Created  3m39s (x2 over 40h)  kubelet  spec.containers{client}: Created container client

  Normal  Started  3m39s (x2 over 40h)  kubelet  spec.containers{client}: Started container client

aryankeshri@Aryans-MacBook-Air-2 simplek8s %
```


-: In the last section we spoke about

how we're going to update an existing object.

We're gonna take that existing config file,

leave the name and the kind the same,

and we can make any other change

we want to to the file, and throw that back into kubectl.

And our expectation is that we are going to update

an existing object as opposed to creating a new one.

So, let's flip on over to our config file.

Remember, our goal right now is to update the existing pod

to use the multi worker image instead of the multi client.

Now, quick note here, remember that the multi worker image

really expects to have a copy of Redis available.

And so it's entirely possible that when we do this update,

something is going to crash,

or I should say the container that gets created might crash

because there is no copy of Redis available.

Nonetheless, I just want to make sure

that we have the ability to update the image

that is being used.

And I'm not really concerned

about getting a pod successfully running.

I just wanna update the image right now. That's all.

All right, so we're gonna open up our code editor.

We're gonna find the client pod file.

We're going to leave the kind the same,

and the name identical as well.

The only thing we're going to update inside of here

is the image that we are using.

So instead of using multi client,

I'm going to use multi worker instead.

We'll leave everything else inside of here identical.

I'm not gonna change the port even though multi worker

doesn't really expose anything on port 3000

or anything like that.

Again, I just wanna update the image

and make sure that we are updating a pod

in place and not creating a new separate pod.

All right, so I made the change to multi worker.

I'm gonna save the file.

I'm gonna flip back over to my terminal.

I'm gonna make sure I'm still inside

my simple K8s directory.

And then I'm going to feed

this updated configuration file back into kubectl

by using that apply command.

Remember, apply is the magic command,

it's how we make any changes to the configuration

of our Kubernetes cluster.

So I'll say kubectl apply-F, client-pod.eml.

And now you'll notice that when we hit enter

and we submit that command,

we do not see something that says like,

oh, I'm gonna update or I'm going to create a new pod,

or anything like that.

Instead it says very specifically,

client pod has been configured.

When you see the message configured, it means

that we've updated the configuration that is applied

to client-pod.

So we should now be able to run that get pods command

and verify that there is still one pod that exists

inside of our entire cluster.

So I'll do kubectl, get pods,

and you'll notice that we now have still just one pod.

It has a name of client pod.

There's one copy, it's running.

There's two restarts currently because we just

caused this thing to be restarted when we updated the image

that it's running.

Now the only thing that's kind of awkward right now

is that we can't really inspect this pod

and verify that it is in fact running the updated image.

So I want to figure out some way that we can actually look

at this very specific pod right here and look

at the different containers that are running inside there

and verify that a hundred percent, no two ways about it.

Yes, we are running the multi worker image.

So in order to inspect this very particular pod right here

we're going to learn a new command on kubectl.

Here's the new command.

Oh, not there. Not there.

Where'd it go? Here we go.

So to get detailed information

about an object inside of our cluster,

we're going to use the kubectl describe command.

Describe is used to print out a ton of information

about a very particular object.

So we'll say kubectl describe.

We'll then pass in an object type,

and then the name of the particular object

that we want to investigate.

Now, just as a quick curiosity here

or something that's kind of interesting.

You can actually omit the object name if you want to.

If you do that, you'll get detailed information

about every different object type

of the specify, or excuse me,

every different object with the specified object type.

But it's a ton of information.

So in general, we usually use the full command

which is kubectl describe, the type and the name,

to just get information about that one particular object.

So let's try that out right now.

Again, we're going to run this command to verify

that the client pod is in fact running that updated image.

So I'll do kubectl, describe, our object type is pod.

So we could simply say pod, either one is fine, pods or pod.

And then we'll put in the name

of that very particular object, which is client-pod.

So that's gonna print out a ton of very detailed information

about that very particular pod.

Down at the very bottom you're gonna see a series

of events right here.

Very frequently, this is going to be

some of the most interesting information.

This is going to be events that are not

logs coming outta the containers, but events that have

occurred over the life cycle of the pod.

So you'll see messages about things that have crashed

or pulling new images or changing images or stuff like that.

Now if we scroll up a little bit higher, there is a ton

of information in here that's kind of outside the scope

of what we want to talk about right now.

But if you scroll up, scroll up, scroll up,

you will see containers.

We have a container with a name of clients.

Recall that client name right there was provided

inside of our configuration file.

So there's name client right there.

And then we're told that the client container,

remember that's the name of the container.

This is kind of misleading 'cause we have this,

these kind of conflicting names all over the place.

But essentially the image is the multi worker image.

That's exactly what we just updated our configuration file

to use.

So without a doubt, updating that configuration file

and leaving the name and the kind identical

found an existing object that was already running

inside of our cluster and it updated it in place

as opposed to trying to create a completely separate pod.

All right, so that's it.

That's a quick example of this entire idea

of declarative versus imperative deployments.

So we like to use declarative as much as possible

and the formula for doing a declarative update

is always gonna be the same thing over and over.

We're gonna go find our original config file.

We're gonna make a change to it,

leaving the type or kind and the name the same.

We'll update some configuration inside the file

and then we're gonna throw it back into kubectl.

And our master is just going to

kind of make the change for us.

And we don't have to worry about

manually printing out a list of running pods

and issuing a command to update a very specific pod

or anything like that.

We're gonna see this declarative approach

throughout this entire course, just nonstop.

It's gonna be all we are doing throughout the entire course.

Update the config file, throw it in kubectl,

verify that in fact everything worked correctly.

Okay, so that's a good example.

Let's take a quick pause right here

and we'll continue in the next section.