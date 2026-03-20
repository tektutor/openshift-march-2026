# Day 2

## Info - Openshift Images
<pre>
image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.27
image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.28
image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.29
image-registry.openshift-image-registry.svc:5000/openshift/wordpress:latest
image-registry.openshift-image-registry.svc:5000/openshift/mariadb:12.0.2
image-registry.openshift-image-registry.svc:5000/openshift/hello:1.0  
</pre>

## Info - You may find this useful
<pre>
https://www.tektutor.org/openshift-ci-cd-with-tekton/
</pre>  

## Info - Container Orchestration Platform Overview
<pre>
- In real world, Container Orchestration Platforms are used to manage our containerized application workloads
- We can only deploy our applications for which we have Container Image
- The Container Orchestration Platform provide all the features necessary to make our application Highly Available
- We could scale up/down individual application based user traffic to your applications
- When required we could use the rolling udpate feature to upgrade your application from one version to the other without any downtime
- also supports in-built, 
  - monitoring features, which could perfom health check, readiness check, and live check on your application
  - it can also repair application
  - load-balancing 
  - exposing your application for internal/external use by way of creating service
- examples
  - Docker SWARM
  - Kubernetes
  - Rancher K3S/RKE2
  - Red Hat Openshift
</pre>

## Info - Docker Swarm
<pre>
- this is Docker Inc's native Container Orchestration Platform
- this supports only Docker Containerized application workloads
- it is very light-weight, easy to setup, easy to learn
- hence, it is ideal for Dev/QA and Proof of Concept purpose or in general for learning
- it is not production grade
</pre>

## Info - Kubernetes
<pre>
- is an opensource Container Orchestration Platform developed by Google in Golang
- supports many Container Runtimes unlike Docker Swarm
- it is very robust, and scales well even for complex environments and heavy applications
- any Container Runtimes, that implements CRI (Container Runtime Interface) are supported by Kubernetes
- production grade
- generally works as a cluster of one or more Master and one or more Worker nodes
- Nodes could be a Physical Server in your local data-center, or could be virtual machine or could an AWS ec2 instance 
  running in AWS
- there are two types of Nodes
  1. Master Node
    - this is where Control plane components will be running
    - generally, user applications will not be scheduled in this type of node
  2. Worker Node
    - user applications will be scheduled in this type of node
- Kubernetes, supports extending Kubernetes API(features) by using Custom Resource Definitions and Custom Controllers
- supports command-line only
- Kubernetes encourages extending Kubernetes API(Features) by adding your Custom Resources in the Kubernetes cluster
- Custom Resources can be adding by adding Custom Resource Definitions (CRD)
- To manage Custom Resources, we also need to support Custom Controller
- Normally Custom Resources and Custom Controllers are packaged as Operators or in some rare cases it could be even package as Helm charts
</pre>


## Rancher
<pre>
- Rancher manages one or more Kubernetes clusters from a Web console or from CLI
- Rancher provides
  - supports CLI and Webconsole
  - User management
  - RBAC
  - External Auth Providers like LDAP, OIDC, etc.,
  - Upgrading/downgrading Kubernetes clusters from Rancher
  - Scale up/down Nodes in a downstream Kuberentes cluster
- developed by Suse
- comes in 2 flavours
  - Community - opensource variant
  - Enterprise variant which requires license
    - comes with world-wide support from Suse
- supports
  - K3S light-weight Kubernetes cluster
  - RKE2 Kubernetes cluster which is highly secured Kubernetes
  - a regular K8s Cluster that we deployed
  - AWS eks managed K8s Cluster
  - Azure aks managed K8s cluster
  - GKE managed K8s cluster from Google Cloud
  - Openshift Cluster
</pre>

## Red Hat Openshift
<pre>
- it is Red Hat's Kubernetes Distribution with many additional features built on top of Opensource Kubernetes
- it supports CLI and Webconsole
- comes with Internal Container Registry, that can used by any nodes in the cluster to pull/push container images to and fro
- in this Container Orchestration Platform, one could deploy application
  - using existing Container images from your custom container application that hosts your application
  - using source code from Version Control like GitHub, etc., this feature is called S2I
- it is an enterprise product, that requires license to use this product
- you can get world-wide support from Red Hat ( an IBM company )
- supports User Management
- supports all the features of Kubernetes with many addition useful features
- some new features added
  - supports DeploymentConfig
  - supports Route
  - supports Project
  - support S2I - BuildConfig & Build, Build Controller
- older version of Openshift supported Docker, starting from version 4.x Openshift only supports CRI-O Container Runtime and Podman Container Engine
- starting from Openshift v4.x, we can only install Red Hat Enteprise CoreOS operating system in master nodes
- starting from Openshift v4.x, we can either install Red Hat Enterprise Linux or Red Hat Enterprise CoreOS in worker nodes
- The Red Hat Enterprise Core OS comes with
  - pre-installed CRI-O Container Runtime and Podman Container Engine
- Red Hat recommends installing RHCOS in both type of nodes i.e master and worker nodes
- In case, we opted to install RHCOS in all master and all worker nodes, upgrading Openshift from one version to other can be
  using Openshift shift client tool oc or from Webconsole
- Openshift also supports Virtualization
  - it is possible to provision Windows or Linux Virtual Machines within Openshift
</pre>

## Info - Openshift Terminologies
<pre>
- Pod
- ReplicaSet
- Deployment
- DeploymentConfig
- ReplicationController
- Job
- CronJob
- StatefulSet
- DaemonSet
</pre>


## Info - Pod
<pre>
- Pod is a group of related containers
- the smallest unit that can be deployed in k8s or Openshift is a Pod
- each Pod, has one secret infra container called pause-container
- why it is called pause-container, is because it does nothing other than providing a network stack
- technically, Pods can container many containers within a single Pod
- IP Address is assigned on the Pod level, not on the container level
- Pod is where containerized application will be running
- each Pod should have just one application Pod
- though technically possible to put many application container per Pod, as a best practice there should only
  one application container per Pod to scale up/down individual applications
- this is a JSON object that is stored and maintained in the etcd database by API Server
- in otherwords, Pod is a logical grouping of containers that belong to an application
</pre>

## Info - ReplicaSet
<pre>
- is a JSON object that is stored and maintained by API Server in etcd database 
- this JSON object captures the below details in high-level
- how many instances of your application should be running within openshift
- which container image must be used to deploy your application container within the pod
- ReplicaSet has one to many Pods
</pre>

## Info - Deployment
<pre>
- is a JSON object that is stored and maintained by API Server in the etcd key/value database
- this JSON object captures the below details
  - name of the deployment( your application )
  - this type of deployment is generally used by stateless applications
  - how many instances of your application should be running within Openshift
  - which container image must be used to deploy your appication container within the pod
- Deployment has one to many ReplicaSets
</pre>

## Info - DaemonSet
<pre>
- DaemonSet is managed by DaemonSet Controller
- DaemonSet Controller detects the total number of nodes available in the cluster and it will create that many Pods automatically
- each Pod will scheduled on one of the nodes 
- this is not used in normal scenarios
- For instance, 
  - to collect performance metrics, there has to be one Prometheus Pod running in each node
  - in this kind of scenario, one can use DaemonSet
</pre>

## Info - Job
<pre>
- any one time activity, we can run as Job
- the activity may run sometime and then it might complete, such application can be deployed as a Job in Kubernetes/Openshift
</pre>

## Info - CronJob
<pre>
- any activity that must be schedule to run a particular day particular time
- this must be repeated daily, weekly or monthly or yearly then it can be deployed as a CronJob
</pre>

## Info - StatefulSet
<pre>
- this is used to deploy stateful applications
- if you wish to deploy a cluster of db server Pod
- assume you wish to create 3 mysql Pods which runs as a cluster
  - ie. when a particular table record is updated in one instance of mysql Pod
  - it automatically syncs with other 2 instances
- StatefulSet Controller guarantees 
  - each Pod that is created for StatefulSet is guaranteed to have a stable name
  - For instance, assuming the statefulset name is mysql then the first Pod will be named mysql-0, 
    the second Pod will be named mysql-1 and so on
  - Even if the mysql-0 Pod or any Pod in Statefulset get deleted the new mysql Pod is assigned the same sticky name
- First mysql-0 Pod behaves like a Master mysql server
  - here read/write is allowed
- Second and Third instance i.e mysql-1 and mysl-2 can be created as a ready-only database
- Second instance will get synchronized pulling updates from mysql-1 and then it will let the mysql-2 get synchronized from mysql-1
</pre>

## Info - ImageStream
<pre>
- Openshift ImageStream represents a folder inside Openshift's Internal Registry 
- We can store multiple versions(tag) of the same image in a single OpenShift Imagestream
</pre>

## References
<pre>
https://medium.com/tektutor/using-metallb-loadbalancer-with-bare-metal-openshift-onprem-4230944bfa35  
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
https://docs.redhat.com/en/documentation/openshift_container_platform/4.21/html/building_applications/deployments
</pre>  

## Info - ClusterIP Service
<pre>
- a ClusterIP Service acts as an internal load balancer that provides a stable, virtual IP address for a set of pods 
- this allows other components in the cluster to communicate with those pods reliably, even as individual 
  pods are created or destroyed
</pre>

## Info - NodePort Service
<pre>
- NodePort service is a configuration in OpenShift/Kubernetes that exposes a service on a specific, 
  static port across every node in the cluster 
- allows external traffic to reach your application by hitting any node's IP address at that designated port
- Kubernetes/Openshift reserves a port available from a specific range in the range 30000–32767 
</pre>

## Info - LoadBalancer Service
<pre>
- a standard way to expose an application to the internet or an external network
- this is normally used in public cloud environment like AWS, Azure, etc.,
- In a on-prem Kubernetes/Openshift, LoadBalancer is not supported out of the box.  We could use
  MetalLB operator to support LoadBalancer Service in a local setup.
- LoadBalancer builds upon the NodePort and ClusterIP types, essentially automating the creation 
  of an external load balancer in your infrastructure to route traffic into the cluster
- Kubernetes/OpenShift communicates with MetalLB to provision load balancer
- MetalLB assigns a unique, stable External IP or DNS name to the service.
- Traffic enters through the external load balancer, which then forwards it to a NodePort on one of the cluster nodes
- From there, it is routed via the ClusterIP to the final Pod. 
</pre>

## Info - Control Plane Components
The below components only runs in the master node, or wherever they run that is called master node
<pre>
1. API Server
2. etcd database
3. Controller Managers
4. Scheduler
</pre>

## Info - API Server
<pre>
- is is a Pod
- is the heart/brain of Kubernetes or Openshift
- it supports REST for every Openshift feature
- API Server stores the Cluster and application states into the etcd key/value datastore
- each time API Server does an update on the etcd database, it will be followed a broadcast event from API Server
</pre>

## Info - etcd
<pre>
- it is a Pod
- it is an opensource independent project 
- it was not developed for Kubernetes or Openshift
- it can be used outside the scope of Kubernetes/Rancher/Openshift
- this is where API Server stores all the information in the form of key/value
- generally 3 instances of etcd databases are required by Openshift to support High Availability and to synchronize data between then
</pre>

## INfo - Controller Managers
<pre>
- it is a Pod
- it is a collection of many Controllers
- Controllers have unrestricted access to monitor specific type of resources created/managed under any project/namespace within openshift
- Controller not only monitors application, they also repair them when required
- Each Controller, manages one type of Kubernetes/Openshift Resource
- Some controller part of Controller Managers are
  - Deployment Controller
  - ReplicaSet Controller
  - Job Controller
  - CronJob Controller
  - Endpoint Controller
  - DaemonSet Controller
  - StatefulSet Controller
- For example
  - Deployment Controller manages Deployment
  - Deployment Controller takes Deployment as the input ensures the desired state and actual state matches
  - ReplicaSet Controller manages ReplicaSet
</pre>

## Info - Scheduler
<pre>
- it is a Pod
- it is responsible to identify a healthy node where a newly created Pod can be scheduled
- the scheduler makes a REST call to API Server with the scheduling recommendataion about a Pod
</pre>

## Info - Helm
<pre>
- is a package manager for Kubernetes and Openshift
- with helm, we can deploy/undeploy/upgrade/downgrade applications into Kubernetes/Openshift
- our custom application manifest files can be packaged as Helm Chart
- Helm Chart is the packaged application - it is like a installed package
- Helm Chart is nothing but a tar ball file that follows a specific folder structure
- Openshift comes with Helm pre-integrated 
</pre>

## Info - Node Affinity
<pre>
- Let's say your application is disk intensive
- It would be preferable if your application is deployed onto a Openshift node that has an SSD faster disk
- this is where the Node Affinity comes into picture
- there are two types of Node Affinity that can be request
  1. Preferred
     - in this case, the scheduler will look for nodes that matches your application criteria if it finds a match
       it will deploy your application Pods on those matching nodes, else it will as usual deploy anywhere
  2. Required
     - in this case, the scheduler will look for nodes that matches your application criteria, if it finds a match
       it will deploy your application pods on those matching nodes, else it put your Pods in pending until it finds such a node
</pre>

## Info - Openshift S2I
<pre>
- Unlike Kubernetes, Openshift apart from deploying applications from readily available or pre-build container images, 
  one could deploy applications from source code from your version control
- Openshift S2I supports many different Strategies
  1. Docker
  2. Source
  3. Custom
  4. Pipeline(Jenkins/Tekton)
  5. Binary(S2I Binary)
</pre>


## Lab - Openshift command-line login 
```
oc login https://api.ocp4.palmeto.org:6443 -u kubeadmin -p W5kfC-Utit3-hc2o3-EvaCy --insecure-skip-tls-verify=true
```

## Lab - Check your lab environment
```
kubectl version
oc version

oc get nodes
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/046009d2-9b6a-4b5e-af44-6386a69420f5" />


## Lab - List the nodes in the cluster
```
oc get nodes
kuebctl get nodes

oc get nodes -o wide
kubectl get nodes -o wide
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/c961132c-ddee-4f58-b694-5ae5fe2bd7fc" />

## Lab - Finding more details about the nodes in the Openshift cluster
```
oc get nodes

oc describe node master01.ocp4.palmeto.org
oc describe node master02.ocp4.palmeto.org
oc describe node master03.ocp4.palmeto.org

oc describe node worker01.ocp4.palmeto.org
oc describe node worker02.ocp4.palmeto.org
oc describe node worker03.ocp4.palmeto.org
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/ecd93d30-5f21-4bde-a329-28f77606e9f7" />
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/f7e882d2-0922-4198-b43b-4b97485e8511" />
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/6b031173-d819-454b-86ff-b5c53ae502de" />

## Lab - Listing the container images present in the Openshift's Internal Container Registry
```
oc get imagestreams -n openshift
oc get imagestream  -n openshift
oc get is -n openshift
```

<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/358b3ab3-3d70-419a-8469-33d433b37ef1" />

## Lab - Find how many versions(tags) are there for nginx image stream in our Openshift Internal Image Registry
```
oc describe imagestreams nginx -n openshift
oc describe imagestream nginx -n openshift
oc describe is nginx -n openshift
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/113f77f4-c3ae-480c-ab5f-937607cde7b5" />
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/bdc089ad-9cf8-47eb-b6f7-14ad1c0779d0" />


## Lab - Managing Openshift projects
List the openshift projects
```
oc get projects
oc get project
```

Creating a new-project
```
oc new-project jegan
```

Switching between projects
```
oc project openshift
oc project jegan
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/b82bfc6e-340f-4ea7-9455-ccd02d009a93" />

Finding the currently active projects
```
oc project
```

Deleting your project only ( This will delete everything inside the project namespace called jegan, currently it is empty )
```
oc get projects | grep jegan
oc delete project jegan
oc get projects | grep jegan
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/ad69cf6f-53ff-4aa2-b6fe-7fc2c8531946" />


## Lab - Let's deploy our first application in Openshift under your project
```
oc new-project jegan
oc create deployment nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.27 --replicas=3
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/2620daf9-d458-42fd-90ca-3cdb56bfc60b" />


List the deployments in your project namespace
```
oc get deployments
oc get deployment
oc get deploy
```

List the replicasets in your project namespace
```
oc get replicasets
oc get replicaset
oc get rs
```

List the pods running in your project namespace
```
oc get pods
oc get pod
oc get po
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/cebeb90f-419a-4ff8-ad2d-7f036a1a91e1" />

List multiple resources with a single command
```
oc get deploy,rs,po
oc get all
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/b2d7abfa-3e0e-4c3a-a517-b673aa290f46" />


Finding the Pod IP and the node where they are running
```
oc get pods -o wide
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/8407e576-1df0-4c4a-9507-aeda90a9b9b9" />

Scale up a deployment
```
oc scale deploy/nginx --replicas=5
oc get pods -w
```

Scale down a deployment
```
oc scale deploy/nginx --replicas=3
oc get pods
```

<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/4cd16617-de30-4a07-92d2-6fc8ee516f25" />

Delete a deployment
```
oc delete deploy/nginx
oc get deploy,rs,po
```

## Lab - What things can go wrong while deploying an application into Openshift

Following things may wrong
<pre>
Image and Registry Issues 
- ImagePullBackOff
  - This is the most common error
  - happens if the image name is mistyped, the tag (like :latest or :v1) doesn't exist
    or the internal registry isn't authenticated properly
- ErrImagePull
  - often points to networking issues or the registry being down or an invalid image

Permission Issues
- Security Context Constraints (SCC)
  - By default, OpenShift forbids containers from running as root. 
  - If your Dockerfile specifies USER root or tries to write to a protected directory, the Pod will crash immediately

Missing ConfigMaps/Secrets
- Your app might look for an environment variable or a database password that hasn't been created in the namespace yet

Wrong Service Ports
- The Service might be listening on port 80, but your application code is actually running on port 8080 
</pre>

Resource & Scheduling Limits
- Insufficient Quotas
  - If your Project (namespace) has a limit on CPU or Memory and your deployment asks for more than what's left,
    the Pod will stay in Pending state.

- OOMKilled (Out of Memory)
  - Your application tried to use more RAM than the limit you set in the deployment YAML
  - The kernel steps in and terminates the process.

- CrashLoopBackOff
  - This is a catch-all. It means the Pod started, failed, and OpenShift is trying to restart it repeatedly
  - This is usually due to an internal application error (like a missing DB connection)

Networking and Routes
- Even if the Pod is Running, the world might not be able to see it.
- Route Misconfiguration
  - OpenShift Route must point to the correct Service
  - If labels don't match, you'll get a 503 Service Unavailable error
- Liveness/Readiness Probe Failures
  - If your Readiness probe is pointing to the wrong path (e.g., /health instead of /actuator/health),
    OpenShift will think the app is broken and won't send it any traffic.


Let's delete the existing nginx deployment
```
oc project jegan
oc delete deploy nginx

oc create deployment nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/invalid-nginx:1.27 --replicas=3
oc get deploy,rs,po
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/d622a99b-ea3a-40d8-9170-754049be68a6" />
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/77d4329e-ffef-494c-afff-6c34d0e06d25" />


## Lab - For quick testing one could use port-forward ( Not for production )
In case you don't have any application deployment under your project, you could deploy one now
```
oc project jegan
oc create deployment nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.27 --replicas=3
oc get deploy,rs,po
```

Troubleshooting ImagePullBack error
```
# You need to edit the image to image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.27 and save & quit
oc edit deploy/nginx
```

Now you can 
```
oc project jegan
oc get pods
oc port-forward nginx-7cb45d7956-5hjwc 8080:8080
oc port-forward nginx-7cb45d7956-5hjwc 0:8080
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/0a821a51-640a-4bb7-b86a-fab59c5c04ff" />
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/c5e084f3-138f-49de-a5f1-764d22ab8cf5" />
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/5792dfc6-a523-40e8-8d68-55bfca3572d3" />

To test the port-forward, you can access from command-line or from one of the web browser on that server
```
localhost:33115
```

## Info - Service
<pre>
- Service represents a group load-balanced pods that belongs a single deployment
- Service helps us expose the application to internal use or external use
- There are 3 types of services supported in Kubernetes and Openshift
  1. ClusterIP ( Internal Service )
  2. NodePort ( External Service )
  3. LoadBalancer ( External Service )
</pre>

## Info - ClusterIP Service
<pre>
- this type of service is useful to restrict access only to the Pods running on the same Openshift/Kubernetes Cluster
- example
  - assume you have a angular application running as a Pod, that needs to retrieve some information from mysql Pod
  - angular application is deployed with name angular, created one angular Pod
  - mysql application is deploy with a name mysql, created one mysql Pod
  - technically, it is possible that angular Pod can connect to mysql Pod directly using the mysql Pod IP ( This is not recommended )
  - Pods are considered ephermeral (temporary), they can come and go at any point of time
  - Instead of accessing the mysql Pod IP, ideally the angular Pod must use the mysql service
  - behind the mysql service, there could 1 or 100s of mysql Pod, which is immaterial for angular application
- Openshift will ensure the service name and Service IP address remains stable, hence we could access the service by its name 
  ( service discovery ) or using the clusterip assigned to the service i.e IP
- the kube-proxy pod that runs in every node will support load-balancing of pods behind the service
</pre>

## Lab - Deploy Hello application in your project
```
oc project jegan
oc create deployment hello --image=image-registry.openshift-image-registry.svc:5000/openshift/hello:1.0 --replicas=3
oc get deploy,rs,po
```

Assuming you have 2 deployments i.e hello and nginx with 3 Pods each

Let's find the label selector used by nginx deployment to identify nginx replicaset
```
oc describe deploy nginx | grep -i "Selector:"
```

This is how nginx deployment will retrieve its replicasets
```
oc get rs -l app=nginx
```

Let's find the label selector used by nginx-c68754854 replicaset to identify its pods
```
oc describe rs nginx-c68754854 | grep -i "Selector:"
```

This is how nginx-c68754854 replicaset will retrieve its pods
```
oc get pods -l app=nginx,pod-template-hash=c68754854
```

## Lab - Creating an internal ClusterIP Service for nginx deployment
```
oc get deploy

oc expose deploy/nginx --type=ClusterIP --port=8080

oc get services
oc get service
oc get svc

oc describe svc/nginx
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/b8938c80-8597-4560-b7c2-c3aa3f67d13b" />


Testing the nginx internal service
```
# This will take us inside one of the hello pod shell
oc rsh deploy/hello

curl http://nginx:8080
curl http://172.30.78.253:8080

exit
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/463b40fb-fb64-4d70-a8a8-e18aa23ef549" />

You could also get inside a specific pod shell
```
oc get pods -l app=hello
oc exec -it hello-9bc9955dc-ljvwr -- sh
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/ac91e902-dd81-4e55-8d6d-78658ef388b9" />


## Lab - Creating a NodePort external service for nginx deployment

Note
<pre>
- Kubernetes/Openshift reserve ports in the range 30000-32767 for NodePort services
- Openshift will automatically assign any one of the available port in the above range
- the NodePort chosen will be opened on all the nodes in your Openshift cluster
- Hence, you can access the NodePort service using any openshift node ip/hostname
</pre>

Let's delete the ClusterIP Internal Service we created for nginx deployment
```
oc get svc
oc delete svc/nginx
```

We can create the NodePort external service for nginx deployment
```
oc expose deploy/nginx --type=NodePort --port=8080
oc get services
oc get service
oc get svc
oc describe svc/nginx
```

Testing the NodePort service
```
curl http://master01.ocp4.palmeto.org:32452
curl http://master02.ocp4.palmeto.org:32452
curl http://master03.ocp4.palmeto.org:32452
curl http://worker01.ocp4.palmeto.org:32452
curl http://worker02.ocp4.palmeto.org:32452
curl http://worker03.ocp4.palmeto.org:32452
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/5ede9997-2591-49a9-b1c8-588c5d48976a" />

## Lab - Creating an external LoadBalancer service for nginx deployment

First, let's delete the NodePort nginx service
```
oc project jegan
oc get svc
oc delete svc/nginx
```

Let's create the loadbalancer service for nginx
```
oc expose deploy/nginx --type=LoadBalancer --port=8080
oc get svc
oc describe svc/nginx
```

<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/0ed4548e-f9c5-4763-be89-bd543a39d60f" />
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/4f8d1601-4efa-4972-9817-8cba05a77ca8" />

Note
<pre>
- By default, LoadBalancer service is not supported on a local openshift setup done in on-prem servers
- Whenever you prefer to use an external Load Balancer like AWS ALB or Azure ALB, then you need to create LoadBalancer Service
- LoadBalancer service is meant to be used in public cloud environment like AWS, Azure, etc.,
  - In AWS, there is a managed Openshift Service called ROSA
    - When we create a LoadBalancer service in ROSA, it will spin-up an ELB/ALB type of LoadBalancer in AWS public Cloud
  - In Azure, there is a managed Openshift Service called ARO
    - When we create a LoadBalancer service in ARO, it will spin-up a Azure Load Balancer in Azure public cloud
- In case, you wish to use LoadBalancer service on your local Openshift setup, then we need to install MetalLB Operator
</pre>
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/e03b4edc-c9da-4215-b852-cdb7cfef7ca8" />
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/de199118-82e3-4f71-98b6-dcfc1be35e76" />
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/b1544993-baa4-4479-b5c2-4e985e92f48a" />
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/c95715ef-690d-42c1-9cd7-619d50c1e043" />
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/7c7a8cbf-8ff2-4e95-a942-9fb9d9fe83ef" />
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/ec3b0e26-3ef7-4edf-bce4-78893bb72526" />

## Demo - Adding a Custom Resource called Training in our openshift cluster

Create a file training-crd.yml
<pre>
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: trainings.tektutor.org
spec:
  group: tektutor.org
  scope: Namespaced
  names:
    kind: Training
    listKind: TrainingList
    plural: trainings
    singular: training
    shortNames:
    - train
  versions:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          training:
            type: string
          duration:
            type: string
          from: 
            type: string
          to:
            type: string  
</pre>


Let's create couple of trainings, create a file named devops-training.yml
<pre>
apiVersion: tektutor.org/v1
kind: Training 
metadata:
  name: devops-training 
spec:
  training: "Advanced DevOps"
  duration: "5 Days" 
  city: "Bengaluru"
  from: "4th July 2026"
  to: "8th July 2026"  
</pre>

## Info - Helm 
<pre>
  
</pre>
