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
