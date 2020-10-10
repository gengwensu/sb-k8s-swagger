# Microservices with Spring Boot and Spring Cloud on Kubernetes Demo Project [![Twitter](https://img.shields.io/twitter/follow/piotr_minkowski.svg?style=social&logo=twitter&label=Follow%20Me)](https://twitter.com/piotr_minkowski)

This project demonstrats aggregated Swagger documentation and features of [Spring Cloud Project](https://spring.io/projects/spring-cloud) for building microservice-based architecture that is deployed on Kubernetes. All the samples may be easily deployed on local Kubernetes single-node cluster - Docker for Mac.

### Setup K8s cluster and nginx-ingress

* Disable other things listening on port 80 / 443 on your Mac:
  * (You might need to `sudo brew services stop launch_socket_server`)
* Install a recent version of Docker for Mac and [enable Kubernetes](https://docs.docker.com/docker-for-mac/#kubernetes)
* Deploy nginx-ingress:

```
kubectl apply -f nginx-ingress/namespaces/nginx-ingress.yaml -Rf nginx-ingress
```
### Usage

1. Build Maven project with using command: `mvn clean install`
1. Build Docker images for each module using command, for example: `docker build -t piomin/employee:1.1 .`
1. Go to `/kubernetes` directory in repository
1. Apply all templates to Minikube using command: `kubectl apply -f <filename>.yaml`
1. Check status with `kubectl get pods`

## Architecture

The sample microservices-based system consists of the following modules:
- **gateway-service** - a module that Spring Cloud Gateway for running Spring Boot application that acts as a proxy/gateway in our architecture.
- **employee-service** - a module containing the first of our sample microservices that allows to perform CRUD operation on Mongo repository of employees
- **department-service** - a module containing the second of our sample microservices that allows to perform CRUD operation on Mongo repository of departments. It communicates with employee-service. 
- **organization-service** - a module containing the third of our sample microservices that allows to perform CRUD operation on Mongo repository of organizations. It communicates with both employee-service and department-service.
- **admin-service** - a module containing embedded Spring Boot Admin Server used for monitoring Spring Boot microservices running on Kubernetes
The following picture illustrates the architecture described above including Kubernetes objects.

<img src="https://piotrminkowski.files.wordpress.com/2018/07/micro-kube-1.png" title="Architecture1">

You can distribute applications across multiple namespaces and use Spring Cloud Kubernetes `DiscoveryClient` and `Ribbon` for inter-service communication.

<img src="https://piotrminkowski.files.wordpress.com/2019/12/microservices-with-spring-cloud-kubernetes-discovery.png" title="Architecture2" >
