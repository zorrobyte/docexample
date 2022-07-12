# Running An Application on the Cluster

You can now deploy any containerized application to your cluster. To keep things familiar, let’s deploy Nginx using _Deployments_ and _Services_ to explore how this application can be deployed to the cluster. You can use the commands below for other containerized applications as well, provided you change the Docker image name and any relevant flags (such as `ports` and `volumes`).

Ensure that you are logged into the control plane node and then and then run the following command to create a deployment named `nginx`:

```
kubectl create deployment nginx --image=nginx
```

A deployment is a type of Kubernetes object that ensures there’s always a specified number of pods running based on a defined template, even if the pod crashes during the cluster’s lifetime. The above deployment will create a pod with one container from the Docker registry’s [Nginx Docker Image](https://hub.docker.com/\_/nginx/).

Next, run the following command to create a service named `nginx` that will expose the app publicly. It will do so through a _NodePort_, a scheme that will make the pod accessible through an arbitrary port opened on each node of the cluster:

```
kubectl expose deploy nginx --port 80 --target-port 80 --type NodePort
```

Services are another type of Kubernetes object that expose cluster internal services to clients, both internal and external. They are also capable of load balancing requests to multiple pods, and are an integral component in Kubernetes, frequently interacting with other components.

Run the following command:

```
kubectl get services
```

This command will output text similar to the following:

```
OutputNAME         TYPE        CLUSTER-IP       EXTERNAL-IP           PORT(S)             AGE
kubernetes   ClusterIP   10.96.0.1        <none>                443/TCP             1d
nginx        NodePort    10.109.228.209   <none>                80:nginx_port/TCP   40m
```

From the highlighted line of the above output, you can retrieve the port that Nginx is running on. Kubernetes will assign a random port that is greater than `30000` automatically, while ensuring that the port is not already bound by another service.

To test that everything is working, visit `http://``worker_1_ip``:``nginx_port` or `http://``worker_2_ip``:``nginx_port` through a browser on your local machine. You will see Nginx’s familiar welcome page.

![](<../.gitbook/assets/image (17).png>)

If you would like to remove the Nginx application, first delete the `nginx` service from the control plane node:

```
kubectl delete service nginx
```

Run the following to ensure that the service has been deleted:

```
kubectl get services
```

You will see the following output:

```
OutputNAME         TYPE        CLUSTER-IP       EXTERNAL-IP           PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1        <none>                443/TCP        1d
```

Then delete the deployment:

```
kubectl delete deployment nginx
```

Run the following to confirm that this worked:

```
kubectl get deployments
```

```
Output
No resources found.
```

\
\
