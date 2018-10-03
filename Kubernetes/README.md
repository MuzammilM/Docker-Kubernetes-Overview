Kubernetes Section 12
	> Object - Objects service different purposes ; running a container, monitoring a container, networking etc.	
	> Node - Physical Computer or virtual machine.
	> Master - Controls what each node does.
	> Cluster - Nodes + Master.

	Request -> Load balancer -> Node1  |
							 -> Node2  | <- Master
							 -> Node3  |


Minikube - 
	> Sets up a tiny cluster for development.
	> minikube start
		> requires virtualbox
		> creates a kubernetes cluster


Important points to remember.
	>Kubernets expects all images to be pre-built.
	>One config file per object we want to create.
	>We have to manually setup networking.

`minikube start`
* Starts a node/virtual machine.
* Node contains a pod.
* We run one or more container inside a pod.

## Pod 
* Container to run a container.
* Grouping of containers with a very similar purpose.

### When do we use a pod?
* Consider a postgres container with two additional containers ; logger and backup-manager.
* Since the two containers need the postgres container it is considered a good practice to run these three within the same pod.
* A pod can run one or more containers inside it.


## Service
* Monitors a running a container.
* Sets up networking inside a kubernetes cluster.
	* ClusterIp
	* NodePort: Expose a container to the outside world for viewing ; it should only be used for dev purposes.
	* LoadBalancer
	* Ingress

Browser Request <-  kube-proxy -> Service -> Pod
        			  		 
Config file breakdown for client-pod.yml:
	> apiVersion :
	> kind : used to indicate the type of object we want to make.
	> spec:
		>containers:
			>name : arbitrary name to give the container a name; like 	        tagging for docker.
					can be used for networking as well.
					logging.
			>image : the docker hub image location.
			>ports :
				>containerPort: on this container we want to expose this specific port.
	> metadata:
		>name: 
			>Used for logging. Name assigned within kubernetes.
		>labels:
			> component: web
				Used to link client-node-pod.yml

## Config file for client-node-pod.yml
	apiVersion: v1
	kind: Service
	metadata:	
  		name: client-node-port
	spec:
 		type: NodePort
 	ports:
  		- port: 3050
   		  targetPort: 3000
    	  nodePort: 31515
 	selector:
  		  component: web



* selector: component: web - To link up the client-node-pod.yml and the  client-pod.yml , "web" is defined under the labels field in client-pod.yml.
* Direct all traffic and expose port 3000 to the outside world.
* targetPort should be identical to the container port .
* port: port:3050 : Other pods that needs multi-client pod
* nodeport : port on which we will access the service on a browser

`kubectl get pods` 
* Get current stats of running pod

## Master (kube-apiserver)
* Monitors current status of nodes in cluster if they are doing the correct thing.
* Gets details of the running process from the user and passes it on to the worker node.
* Each worker node has a docker daemon running.
* Each worker node downloads the image cache and based on the orders passed onto it by the master they start running the docker images.
* Once started they inform the master and update the status with him.
* We never have to "login" to a node and issue commands ; the master does that for us.

## Points to remember
* Kuberenetes is a systemto deploy containerized apps.
* It does not build containers , just deployes ready made containers on nodes.
* It decideds the location of each container.
* To deploy we update the desired state of the master with a config file.
* Master works to meet your desired state.

* Types of deployment associated with kuberenetes
	* Imperative deployments : do exactly these steps at the container setup.
	* Declarative deployments : this is what the setup should look like ,make it happen.


### Imperative Approach

* Get current status of containers.
* Tell the containers how they should get updated.

### Declarative Approach 

* Update the config file keeping the Name(metadata) and Kind the same.
* Apply it with kubectl.
* Master will handle the rest.
* We then use "describe" option to verify if the deployment has been done correctly.

## Deployment 
Change spec: port: field .
* The Pod "client-pod" is invalid: spec: Forbidden: pod updates may not change fields other than `spec.containers[*].image`, `spec.initContainers[*].image`, `spec.activeDeadlineSeconds` or `spec.tolerations` (only additions to existing tolerations)

Except those , youre not allowed to change. Thus the need for "Deployment" kind was created.
* Maintains a set of identical pods, ensuring that they have the correct config and that the right number exists
* Deployments will create the necessary pods.

### Deployment Configuration file breakdown

* replicas: The number of pods that should be created.
* selector: The file tells the master to create a pod and the deployment then looks out for component: web.
* template: Every pod that is created will have the label component: web.p


`kubectl get deployments`