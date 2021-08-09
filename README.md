# SOME RARELY USED COMMANDS

Get a list of all the API resources and if they can be in a namespace
```kubectl api-resources --namespaced=true | head```
```kubectl api-resources --namespaced=false | head```

Get all the pods in our cluster across all namespaces. Right now, only system pods, no user workload.
You can shorten --all-namespaces to -A
```kubectl get pods --all-namespaces```
```kubectl get pods -A```

Get all the resource across all of our namespaces
```kubectl get all --all-namespaces```
```kubectl get all -A```

Creating a resource imperitively
```kubectl run hello-world-pod \```
```    --image=gcr.io/google-samples/hello-app:1.0 \```
```    --generator=run-pod/v1 \```
```    --namespace playground1```

A list of the objects available in that API Group
```kubectl api-resources --api-group=apps```

We can use explain to dig further into a specific API Object 
Check out KIND and VERSION, we'll see the API Group in the from group/version 
Follow the version specified in THAT deprecation warning. Ah...the one we should use.
```kubectl explain deployment --api-version apps/v1 | head```


---

Start up a kubectl proxy session, this will authenticate use to the API Server
Using our local kubeconfig for authentication and settings

```kubectl proxy &```
```curl http://localhost:8001/api/v1/namespaces/default/pods/hello-world | head -n 20```
```fg```
```ctrl+c```

Start kubectl proxy, we can access the resource URL directly.
```kubectl proxy &```
```curl http://localhost:8001/api/v1/namespaces/default/pods/hello-world/log ```

Kill our kubectl proxy, fg then ctrl+c
```fg```
```ctrl+c```

---

Selector for multiple labels and adding on show-labels to see those labels in the output
```kubectl get pods -l 'tier=prod,app=MyWebApp' --show-labels```
```kubectl get pods -l 'tier=prod,app!=MyWebApp' --show-labels```
```kubectl get pods -l 'tier in (prod,qa)'```
```kubectl get pods -l 'tier notin (prod,qa)'```

Edit the label on one of the Pods in the ReplicaSet, change the pod-template-hash
```kubectl label pod PASTE_POD_NAME_HERE pod-template-hash=DEBUG --overwrite```

Delete our labels from Nodes/Any Objects
```kubectl label node c1-node2 disk-```
```kubectl label node c1-node3 hardware-```

---

Getting name of a Pod from a list of Pods
```PODNAME=$(kubectl get pods --selector=app=hello-world -o jsonpath='{ .items[0].metadata.name }')```
```echo $PODNAME```

Accessing our Pod's application directly, without a service and also off the Pod network.
```kubectl port-forward PASTE_POD_NAME_HERE 80:8080```

Let's do it again, but this time with a non-priviledged port
```kubectl port-forward PASTE_POD_NAME_HERE 8080:8080 &```

We can point curl to localhost, and kubectl port-forward will send the traffic through the API server to the Pod
```curl http://localhost:8080```

Kill our port forward session.
```fg```
```ctrl+c```

---

Create our secret that we'll use for our image pull...from our docker config.json
```kubectl create secret generic private-reg-cred \```
```    --from-file=.dockerconfigjson=/home/aen/.docker/config.json \```
```    --type=kubernetes.io/dockerconfigjson```


If needed we can specify this explicitly using the following parameters
````kubectl create secret docker-registry private-reg-cred \````
````    --docker-server=https://index.docker.io/v1/ \````
````    --docker-username=$USERNAME \````
````    --docker-password=$PASSWORD \````
````    --docker-email=$EMAIL````

---

Applying Taint to a Node
```kubectl taint nodes c1-node1 key=MyTaint:NoSchedule```

