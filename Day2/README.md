# Day 2

## Info - Openshift Images
<pre>
image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.27
image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.28
image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.29
image-registry.openshift-image-registry.svc:5000/openshift/wordpress:latest
image-registry.openshift-image-registry.svc:5000/openshift/mariadb:12.0.2
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

## Lab - Check your lab environment
```
kubectl version
oc version

oc get nodes
```
<img width="1906" height="1120" alt="image" src="https://github.com/user-attachments/assets/046009d2-9b6a-4b5e-af44-6386a69420f5" />
