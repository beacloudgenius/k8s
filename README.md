Scalable Microservices with Kubernetes

* Provision Kubernetes
* Deploy and manage Docker containers using kubectl

Kubernetes Version: 1.4.6

    dnf install -y jq

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

### Creating Pods

Explore config file

    cat pods/monolith.yaml

Create the monolith pod

    kubectl create -f pods/monolith.yaml

Examine pods

    kubectl get pods

It may take a few seconds before the monolith pod is up and running as the monolith container image needs to be pulled from the Docker Hub before we can run it.

Use the kubectl describe command to get more information about the monolith pod.

    kubectl describe pods monolith


Interacting With Pods

SHELL 1

set up port-forwarding

    kubectl port-forward monolith 5080:80

SHELL 2

Open new Cloud Shell session 2

    curl http://127.0.0.1:5080

    curl http://127.0.0.1:5080/secure

Cloud shell 2 - log in


    TOKEN=$(curl --cacert ./ca.pem http://localhost:5080/login -u user | jq -r '.token')
    Enter host password for user 'user':
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   222  100   222    0     0   2393      0 --:--:-- --:--:-- --:--:--  2439

    echo $TOKEN
    eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVzZXJAZXhhbXBsZS5jb20iLCJleHAiOjE0ODA5MDg1MjUsImlhdCI6MTQ4MDY0OTMyNSwiaXNzIjoiYXV0aC5zZXJ2aWNlIiwic3ViIjoidXNlciJ9.JQIsbDRxxai1nxlYjGLfsW6V_Pe19kchJpE0PGP4Z-A

    curl --cacert ./ca.pem -H "Authorization: Bearer $TOKEN" http://localhost:5080/secure
    {"message":"Hello"}

    curl --cacert ./ca.pem http://localhost:5080/secure
    authorization failed

View logs

    kubectl logs monolith
    kubectl logs -f monolith

SHELL 3

In Cloud Shell 3

    curl http://127.0.0.1:5080

SHELL 2

    Exit log watching (Ctrl-C)

You can use the kubectl exec command to run an interactive shell inside the monolith Pod. This can come in handy when you want to troubleshoot from within a container:

    kubectl exec monolith --stdin --tty -c monolith /bin/sh

For example, once we have a shell into the monolith container we can test external connectivity using the ping command.

    ping -c 3 google.com

When you’re done with the interactive shell be sure to logout.

    exit
