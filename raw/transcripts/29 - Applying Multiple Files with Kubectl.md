Instructor: Rather than moving onto our next config file,

I think that it would be kind of nice

to take these two config files we just put together

and load them up into Kubernetes just to make sure

that everything that we've done so far is on the level

and that we don't have any typos

or anything like that in there.

Now I do want to remind you that when we load up

both these config files through the kubectl apply command,

we're not going to be able to very easily

test out the multi-client deployment

or the client deployment right here,

just because the service that is governing access to it

is not allowing outside traffic.

So we will not be able to test this out

inside of our browser just yet,

but we can at least start up the deployment

and look at it in kubectl and say,

"Okay, our pods are actually running."

So let's get to it.

We're going to load up both these config files right here.

Now, the first thing I want to do is to make sure

that we delete our old deployment

the previous deployment that we had put together

as we were first learning a lot of this stuff.

So to delete the old deployment,

I'm going to list it out with kubectl,

get deployments, and then I will delete it with kubectl,

delete client deployment

or see me delete deployment, client deployment.

There we go, again we list out

delete the type of object we want to delete

and then the name of the thing that we want to delete.

So I'll run that and now I can do a kubectl

get deployments and no resources are found.

Now don't forget, we had also created a service as well.

It was a note port that was providing access

to the set of pods that were created by that deployment.

So to get a list of all the services we have created

I'll do kubectl get services.

Now we can very easily see that

yes there is in fact the client node pod there.

So we can delete this thing in a very similar fashion.

I'll do kubectl delete service client node pod, like so.

Oops! Typo there.

There we go.

So now if I do another get services,

I'm only gonna see the original Kubernetes service.

And again, we're not touching that.

That's something internal to Kubernetes

and we don't need to mess around with it at all.

All right, so now we're ready to apply

those two config files.

Now, we could use the same technique that we had previously

used for loading up config files where we did something

like kubectl ,apply dash f,

and then remember we're currently inside the complex folder,

but all of our config files are inside of K8s.

So we could do something like K8s

and then what was the first one called?

Client deployment.

But there's actually a little shortcut

that we can use anytime that we want to apply a group

of configuration files.

So rather than spelling out the entire path right here

I'm going to instead confirm that the K8s directory

is right there.

And then I can apply every file inside there

with apply dash f.

And I'm just gonna provide the K8s directory

when I do this kubectl is going to look into that folder.

It's gonna find every config file in there

and it's going to apply all of them at the same time.

So if I run that

I'm going to see that we have both IP service created

and in the case of the deployment file,

it looks like I probably made a little typo

at some point in time.

Let's check that out really quickly.

See inside there I said metadata label,

Oops! it should be labels, plural like so.

So I'm just gonna make that very quick change.

And then if I try applying that thing again

I get both them created.

Well, at least the first one was unchanged

but the second one was created.

So now we can do a kubectl, get deployments

and we'll see that we have three pods created

tied to our client deployment.

And of course, if we do a get pods

we'll see all three of those different pods running.

Finally, we can do a kubectl, get services

and we'll see that we have the cluster IP service created

for our client as well.

Cool, so we're going to make use of this kubectl

apply on a entire directory quite a bit

since you and I have like 11 different configuration files

at least to manage throughout this application.

All right, so let's take a quick pause right here

and we'll continue in the next section.

```bash
aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl get deployments

NAME                READY   UP-TO-DATE   AVAILABLE   AGE

client-deployment   5/5     5            5           4h48m

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl delete deployment client-deployment 

deployment.apps "client-deployment" deleted from default namespace

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl get deployments                    

No resources found in default namespace.

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl get services

NAME               TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE

client-node-port   NodePort    10.102.169.232   <none>        3050:31515/TCP   45h

kubernetes         ClusterIP   10.96.0.1        <none>        443/TCP          29d

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl delete service client-node-port

service "client-node-port" deleted from default namespace

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl get services                   

NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE

kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   29d

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl apply -f k8s                        

service/client-cluster-ip-service created

Error from server (BadRequest): error when creating "k8s/client-deployment.yaml": Deployment in version "v1" cannot be handled as a Deployment: json: cannot unmarshal string into Go struct field Container.spec.template.spec.containers.ports of type v1.ContainerPort

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl apply -f k8s

service/client-cluster-ip-service unchanged

Error from server (BadRequest): error when creating "k8s/client-deployment.yaml": Deployment in version "v1" cannot be handled as a Deployment: json: cannot unmarshal string into Go struct field Container.spec.template.spec.containers.ports of type v1.ContainerPort

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl apply -f k8s

service/client-cluster-ip-service unchanged

deployment.apps/client-deployment created

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl get deployments

NAME                READY   UP-TO-DATE   AVAILABLE   AGE

client-deployment   3/3     3            3           14s

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl get services

NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE

client-cluster-ip-service   ClusterIP   10.106.232.84   <none>        3000/TCP   2m4s

kubernetes                  ClusterIP   10.96.0.1       <none>        443/TCP    29d

aryankeshri@Aryans-MacBook-Air-2 complex-gh % kubectl get pods    

NAME                                 READY   STATUS    RESTARTS   AGE

client-deployment-576bc76988-d7brf   1/1     Running   0          61s

client-deployment-576bc76988-mm5gb   1/1     Running   0          61s

client-deployment-576bc76988-nch6x   1/1     Running   0          61s

aryankeshri@Aryans-MacBook-Air-2 complex-gh %
```