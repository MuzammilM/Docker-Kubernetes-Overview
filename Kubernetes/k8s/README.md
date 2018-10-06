# Test application based on kubernetes

## Run it on your own hardware via
### Install kubectl
`curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && sudo chmod +x kubectl && sudo mv kubectl /usr/local/sbin/kubectl`

### Install minikube
`curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.29.0/minikube-linux-amd64 && chmod +x minikube && sudo cp minikube /usr/local/bin/ && rm minikube`

### Start minikube
`minikube start`

## Overview of cluster application

![Workflow Diagram](https://github.com/MuzammilM/Docker-Kubernetes-Overview/blob/master/Kubernetes/k8s/Workflow.PNG)


Postgres PVC
Postgres volume claim
Should anything happen to the pod , the pod is created ; thus deleting all the data stored within the pod. We therefore map the volume of the container to a volume on the host machine.
![PVC gram](https://github.com/MuzammilM/Docker-Kubernetes-Overview/blob/master/Kubernetes/k8s/PostgresPVC.PNG)asdasasdasda

`kubectl apply -f k8s/`

* Applies all configuration files present within this folder.

`kubectl get pods`

`kubectl get deployments`

`kubectl logs <containername>`

## Combining multiple configuration files
* We can combine multiple configuration files into a single file by appending "---" at the end and writing a new configuration file after.
* Check **server-config.yaml** for the example.


# DISCLAIMER
**Based off of the course created by stephengrider on udemy.**
