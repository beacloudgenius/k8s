Scalable Microservices with Kubernetes

* Provision Kubernetes
* Deploy and manage Docker containers using kubectl

Kubernetes Version: 1.4.6

## Course Description

Kubernetes is all about applications and in this course you will utilize the Kubernetes API to deploy, manage, and upgrade applications. You will use an example application called `app` to complete the labs.

App is an example [12 Factor](https://12factor.net/) application. During this course you will be working with the following Docker images:

* [cloudgenius/monolith-example](https://hub.docker.com/r/cloudgenius/monolith-example) - Monolith includes auth and hello services.
* [cloudgenius/auth-example](https://hub.docker.com/r/cloudgenius/auth-example) - Auth microservice. Generates JWT tokens for authenticated users.
* [cloudgenius/hello-example](https://hub.docker.com/r/cloudgenius/hello-example) - Hello microservice. Greets authenticated users.
* [nginx](https://hub.docker.com/_/nginx) - Front end to the auth and hello services.

## Links

  * [Kubernetes](http://kubernetes.io/)
  * [gcloud Tool Guide](https://cloud.google.com/sdk/gcloud)
  * [Docker](https://docs.docker.com)
  * [etcd](https://coreos.com/docs/distributed-configuration/getting-started-with-etcd)
  * [nginx](http://nginx.org)


## nginx

```
kubectl run nginx --image=nginx:1.10.0
deployment "nginx" created

kubectl get pods
NAME                    READY     STATUS    RESTARTS   AGE
nginx-452959208-3iavq   1/1       Running   0          19s

kubectl expose deployment nginx --port 80 --type LoadBalancer
service "nginx" exposed

kubectl get services
NAME         CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
kubernetes   10.247.0.1       <none>        443/TCP   1d
nginx        10.247.118.175   <pending>     80/TCP    58s

kubectl delete service nginx
service "nginx" deleted

kubectl get deployments
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx     1         1         1            1           2m

kubectl delete deployment nginx
deployment "nginx" deleted
```


### Cheat Sheet

http://kubernetes.io/docs/user-guide/kubectl-cheatsheet/
