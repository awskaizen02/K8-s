* Microservice : Independent deployable components.
Every small service -> 1 Application -> Docker Image

* Orchestration = manage (create,scaling,deleting,updating,monitor,etc..)

* scalable = scale up/down -> increase/decrease contaners .

* resilient = maintain erros/issues.. (manage the count)

[K8s Document] (https://kubernetes.io/)

* k8s is a Orchestration, implemented using GoLang. Maintained by CNCF

* container runtime s/w : docker,crio,rkt(rocket),containerd..etc.

## K8s is a Orchestration tool, not a container management tool

# Pod: it is a abstration over container.

# Pod : multiple container but not same type.
	 * search-Container + LogContainer +SecurityContainer 
	 * Actual Container + Helper Containers

## what pod?
	* Independent of container techonology.	 
	* Hold multiple containers.
# Every Worker node containes one agent.
	* This agent s/w is called as kubelet.
	* It will connect with master/control plane to get the work.
# KubeProxy manage networking with cluster.
	* It helps to cluster to create, manage networking,request, Loadbalane,etc..
	
# ectd: It is a key-val based storage.
	* Contains cluster data, like no of worker nodes,pods,namespaces,volumes..etc

# Controller-Manager: Does actual work.	

# Scheduler	: check the current state and compare with expected state and tell what is missing and what todo to API server.  (Hey! API-Server, create continer-02 at WorkerNode-01)

# Kubectl: 
 * CLI, to communicate with controlplane
 
 
 
 Minikube // in local machine for pratice purpose : 1 Node cluster - Test/dev work.

Kubeadm/ managed cluster: 1 master + n- worker

Cloud
	EKS 
	GKS
	AKS
 
 Master   - cloud will take care
 worknode - 
 
 
#Container Runtime:
*	cri-o,rkt,docker,containerd,etc...
	
## Objects in K8s:-

Pod : 1 or N container.

Flipkart Application Microservice design
Search.waar -> Docker Image -> DockerHub -> k8s [Searcg Pod]
Payment.war (otp.war) -> 2 Docker Image -> DockerHub -> k8s [ PaymentPod --> PaymentContainer + OTP Container]


* By using command (kubectl run)
* By using  YAML Configuration

```
apiVersion: 		// version
kind:       		// what kind of file
metadata:   		// adation information Name & Label  
spec:       		// spec changes depends on kind

```
# https://minikube.sigs.k8s.io/docs/start/

### MINIKUBE INSTALLATION IN AMAZON LINUX 2023:

	##Type: t3.medium (2-CPU and 4-GB RAM) 
	
	#[STOP INSTANCE AFTER YOUR USAGE IS DONE, IT IS A BILLABLE INSTANCE]
	
* Install Docker
```
$ sudo yum update -y
$ sudo yum install docker -y

$ sudo systemctl start docker  # Start and Enable Docker
$ sudo systemctl enable docker

$ sudo usermod -a -G docker $USER  # Add Current user to docker group
$ sudo systemctl status docker
```
* Exit from current session and re-connect
```
$ exit
Press 'R' to re-connect 
```
* Download and Install Kubectl Client #  https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/                                                                                                                                                       
```
$ sudo curl --silent --location -o /usr/local/bin/kubectl https://dl.k8s.io/release/$(curl --silent --location https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
$ sudo chmod +x /usr/local/bin/kubectl
$ kubectl version --client
```
* Download and Install Minikube Software # https://minikube.sigs.k8s.io/docs/start/
```
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
$ sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
* Start Minikube software
```
$ minikube start
```
* Check Installation status using these commands
```
$ minikube status
$ minikube version
$ kubectl version --short
$ kubectl cluster-info
$ kubectl get nodes
```
9. If you stop and start your EC2 Instance then you need to start again Minikube software
# Check Status (it may show Stopped message)
```
$ minikube status
```
# Wait 1-2 minutes to start
```
$ minikube start
```
kubectl get pods
kubectl get Po

Kubectl get svc

1. create a new Pod
$ kubectl run <pod-name> --image <image-name> --port <containier-port>
```
kubectl run first-pod --image nginx --port 80
kubectl delete pod <pod-name>
```
#mypod.yml  # https://kubernetes.io/docs/concepts/workloads/pods/
```
apiVersion: v1
kind: Pod
metadata:
  name: first-pod
spec:
  containers: 
    - name: test-web-app
	  image: nginx
	  ports: 
	    - containerPort: 80
 
```
# https://kubernetes.io/docs/reference/kubectl/quick-reference/

```
create using YAML file

kubectl create -f <fle.yml>
* kubectl create -f  mypod.yml

create if not exist, els modify based on new file

* kubectl apply -f mypod.yml

* kubectl get pods -o wide

view issues/events 

* kubectl describe pod <pod-name>
* kubectl get pod first-pod -o yaml


*  kubectl get pod -A

* View Logs of a pod
$ kubectl logs <pod-name>
$ kubectl logs first-pod

*  kubectl exec first-pod -- cat /etc/os-release

* kubectl port-forward first-pod 80:8080 --address 0.0.0.0 &
* kubectl get pod <pod-name> -o json
* kubectl get pod <pod-name> -o jsonpath='{.status.podIP}'

* Delete pod
$ kubectl delete pod <pod-name>
$ kubectl delete pod first-pod

kubectl delete -f pod.yml

```

###Pods:
* In Kubernetes, a pod is the smallest and simplest unit of deployment. 
* It represents a single instance of a running process within a cluster.
Pods are used to encapsulate one or more containers, along with shared resources and configuration that are co-located and share the same context.
*	Container encapsulation: Pods provide a way to encapsulate and manage one or more containers together as a single unit. Containers within a pod share the same network namespace, IP address, and ports. They can communicate with each other using localhost.

*	Atomic scheduling: Pods are the basic unit of scheduling in Kubernetes. When scheduling pods, Kubernetes ensures that all containers within a pod are scheduled on the same node. This helps ensure that containers in the same pod have low-latency communication and can share resources efficiently.

*	Shared resources: Pods share certain resources, such as storage volumes and IP addresses. Each pod has its own unique IP address within the cluster, enabling direct communication between pods using IP-based protocols.

*	Lifecycle coordination: Pods provide a way to coordinate the lifecycle of multiple containers. When a pod is created, all containers within the pod are started simultaneously. Similarly, when a pod is deleted, all containers within the pod are terminated together.

*	Single service endpoint: Pods are not directly exposed outside the cluster. Instead, they are typically used as building blocks for higher-level abstractions like services and deployments. Services provide a stable endpoint to access pods, allowing external traffic to be load-balanced across multiple pod instances.

*	Scalability and resilience: Pods can be horizontally scaled by creating multiple replicas of the same pod template. Kubernetes handles the distribution of replicas across nodes and ensures load balancing. If a pod fails, Kubernetes can automatically reschedule and recreate the pod to maintain the desired replica count.

*	Metadata and labels: Pods can be labelled and annotated with metadata, allowing for easy grouping, selection, and management. Labels are key-value pairs attached to pods, enabling flexible pod selection for operations like scaling, deployment, and networking.
