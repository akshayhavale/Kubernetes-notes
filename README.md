# Kubernetes-notes


Kubernetes is container orchestrating tool

Advantages Of Kubernetes
1. High Availability or no downtime
2. Scalability or high performance 
3. disaster recovery 

Kubernetes arachitecture 

Master Node ->many Worker nodes

1. Each Worker Node has Kublet process running on it
2. On worker node different application containers running on it

3. Then master node what it consists
a)  API server -> Which is also container and entry point to the kubernetes cluster
b) CONTROLLER MANAGER ->  Keeps tracks of what's happening in cluster
c) Scheduler -> ensures pods placement i.e based on work loads scaling server resources on each node based on available resources
d) etcd -> Key value storage, basically holds current state of kubernetes cluster
		Which has configuration data and status data
		From etcd snapshot we can recover whole cluster 

4. VIRTUAL NETWORK-> which helps to communicate master node and worker nodes.

WORKER NODE
--------------------
PODS. SERVICES and INGRESS

NODE -> Simple server
----------------------------------

POD -> Smallest unit of kubernetes, POD is abstraction over container, creates running running environment or layer on container, Usually one application per pod 

How each pods are communicate with each other -> by virtual network, which means by IP address
i.e each pod has its own IP address, its not public one so container can communicate using that IP address in NODE

SERVICE  -> Basically static IP address or Paramanent IP address that can be attached to each pod
		-> it also act like load balancer
		-> helps to connect between different PODS i.e on replicas , For replicas we won't create a new pods, for that we define blueprints of pods

NOTE -> life cycle of PODS and SERVICES are not connected i.e if Pod dies also Service wont Die that is IP address stay
		so dont have to change end points, 


Now App should be accessible from browser for this ?
--------------

For to access the app through external service we need to create EXTERNAL SERVICE

But database should not expose to external so that we create INTERNAL SERVICE

-> SO for exposed services for external the IP address and PORT given URL should not provide as end product
for that we need INGRESS

NOW the request comes external First gos INGRESS then to the SERVICE

CONFIG MAP AND SECRET
------------------------------

CONFIG MAP : - Basically helps for externalise the configurations, containes the database url's etc or other services url which                                    we used for communication
			:- Here what ever the data stored is in plain text format

Now database pod URL's can be stored in CONFIG MAP but what about passwords and username

SECRET :- just like config map but it used to store secret data, but here stores data in base64 encoded format


VOLUMES
--------------

-> For database data to store for long time for the we need to use Volumes
-> it basically attaches physical storage of hard drive i.e can be local machine or it could be remote storage
	outside the cluster
	
NOTE :- K8 doesn't manages explicitly data persistance  

DEPLOYEMENT AND STATEFUL SETS
-------------------------------------------------


:- The pods blueprint for creating Replicas is called as deployements
:- Abstraction of PODS

NOTE :- Can't be replicate DATABASES by DEPLOYEMENT, BECAUSE IT HAS state 

:- Here which database should write and read should maintain for data consistency, for that machinism  
we have another component STATEFUL SET

So MYSQL, MONGO, ELASTIC SERACH SHOULD BE CREATED BY STATEFUL SETS NOT BY DEPLOYEMNTS 


MINIKUBE
---
1. For install 
brew isntall hyperkit

2. For to create Cluster
 minikube start --vm-driver=hyperkit
this is telling munukube to use hypervisor install hyperkit for to create cluster

3. TO GET NODE INFO
kubectl get nodes
or
minikube status
4. to check version
kubectl version

5. TO CHECK SERVICE
kubectl get service

6. TO CREATE POD
note :- We directly wont create POD we create deployements 
because deployments are abstraction of pods

kubectl create deployment NAME --image=image [--dry-run] [options]

ex:- kubectl create deployment NAME nginx-deplymnt --image=nginx:latest


7. TO CHECK DEPLOYMENT
kubectl get deployment

8. TO CHECK KUBECTL LOGS
kubectl logs PODNAME

9. TO CEHCK PODS
   kubectl get pod

10. TO CHECK POD EXECUTION
kubectl exec -it PODNAME

11. TO CHECK REPLICA SET
kubectl get replicaset

12. TO DELETE DEPLOYEMENT
kubectl delete deployment DEPLOYEMENTNAME

13. TO APPLY CONFIGURATION FILE WITHOUT USING COMMANDS TO CREATE PODS
kubectl apply -f FILENAME.yml

14. TO CEHCK CLUSTER IS THERE ANYTHING
kubectl get all

15. TO GET BASE64 VALUE
~echo -n 'SOMETHINGYOUWANTTOCONVERT' | base64

16. TO CHECK DEPLOYEMNT PROGRESS
kubectl describe pod PODNAME

17. ALSO TO CHECK SERVICE 
kubectl describe service SERVICENAME

18. SO TO NEED TO CHECK ALL SERVICE DEPLOYEMENTS RELATED TO POD
kubectl get all | grep mongodb

19. TO make kubernetes services to be externalised use loadbalanced
