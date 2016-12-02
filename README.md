Scalable Microservices with Kubernetes

* Provision Kubernetes
* Deploy and manage Docker containers using kubectl

Kubernetes Version: 1.4.6

## Get the code

    rm -rf ~/go
    mkdir -p ~/go
    mkdir -p $GOPATH/src/github.com/beacloudgenius
    cd $GOPATH/src/github.com/beacloudgenius
    git clone https://github.com/beacloudgenius/k8s
    cd k8s/app/monolith
    go get -u
 
 
## Course Description

Kubernetes is all about applications and in this course you will utilize the Kubernetes API to deploy, manage, and upgrade applications. You will use an example application called `app` to complete the labs.

App is an example [12 Factor](https://12factor.net/) application. During this course you will be working with the following Docker images:

* [cloudgenius/monolith-example](https://hub.docker.com/r/cloudgenius/monolith-example) - Monolith includes auth and hello services.

    cd app/monolith
    go get -u
    go build --tags netgo --ldflags '-extldflags "-lm -lstdc++ -static"'


* [cloudgenius/auth-example](https://hub.docker.com/r/cloudgenius/auth-example) - Auth microservice. Generates JWT tokens for authenticated users.
* [cloudgenius/hello-example](https://hub.docker.com/r/cloudgenius/hello-example) - Hello microservice. Greets authenticated users.
* [nginx](https://hub.docker.com/_/nginx) - Front end to the auth and hello services.

## Links

  * [Kubernetes](http://kubernetes.io/)
  * [gcloud Tool Guide](https://cloud.google.com/sdk/gcloud)
  * [Docker](https://docs.docker.com)
  * [etcd](https://coreos.com/docs/distributed-configuration/getting-started-with-etcd)
  * [nginx](http://nginx.org)
