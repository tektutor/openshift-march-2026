# Day 3

## Info - Openshift Images
<pre>
image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.27
image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.28
image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.29
image-registry.openshift-image-registry.svc:5000/openshift/wordpress:latest
image-registry.openshift-image-registry.svc:5000/openshift/mariadb:12.0.2
</pre>

## Lab - Creating external route for your nginx deployment
```
oc project jegan
oc get deploy

oc expose deploy/nginx --port=8080
oc expose svc/nginx

oc get route
curl http://nginx-jegan.apps.ocp4.palmeto.org
```

## Info - Recommended Application source directory Structure
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/ceafe5b6-0a88-42ea-ac45-a265f37050aa" />

## Lab - Deploying nginx in declarative style
```
oc project jegan

oc create deployment nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.27 --replicas=3 -o yaml --dry-run=client
oc create deployment nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.27 --replicas=3 -o yaml --dry-run=client > nginx-deployment.yml

oc create -f nginx-deployment.yml --save-config
oc get deploy,rs,po
```

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/9e0c5982-704a-4551-9d15-1087d8a70b69" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/4f028306-30b4-4b6d-b09b-1ca0a8676f5e" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/a6e6ce18-fc8d-4fb4-8762-85fdc921e974" />

## Lab - Creating a ClusterIP Internal Service for nginx deployment in declarative style
```
oc project jegan
oc expose deploy/nginx --type=ClusterIP --port=8080 -o yaml --dry-run=client
oc expose deploy/nginx --type=ClusterIP --port=8080 -o yaml --dry-run=client > nginx-clusterip-svc.yml

oc create -f nginx-clusterip-svc.yml

oc get svc
oc describe svc/nginx
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/29b6f25e-e9d2-43c3-b7f7-3b388a016361" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/32d2acfc-fbe1-49f1-afe9-92c3a57fe7af" />

## Lab - Testing the ClusterIP Service using curl on the Hello Pod
```
oc create deploy hello --image=image-registry.openshift-image-registry.svc:5000/openshift/hello:1.0 -o yaml --dry-run=client
oc create deploy hello --image=image-registry.openshift-image-registry.svc:5000/openshift/hello:1.0 -o yaml --dry-run=client > hello-deployment.yml

oc create -f hello-deployment.yml --save-config
oc get pods
oc get svc
oc rsh deploy/hello
curl http://nginx:8080
exit
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d86cb2df-3bb5-4dc0-bbb6-abc2ecdb7b5d" />

## Lab - Creating a NodePort external service in declarative style

First let's delete the existing nginx service
```
oc get svc
oc delete -f nginx-clusterip-svc.yml
oc get svc
```

Let's create the nodeport external service for nginx deployment
```
oc expose deploy/nginx --type=NodePort --port=8080 -o yaml --dry-run=client
oc expose deploy/nginx --type=NodePort --port=8080 -o yaml --dry-run=client > nginx-nodeport-svc.yml

oc apply -f nginx-nodeport-svc.yml
oc get svc

curl http://master01.ocp4.palmeto.org:32365
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c5c5a428-e787-46a3-bde6-9b9556b040fe" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d0048801-e559-489a-ac67-cffd1b2f827b" />


## Info - Helm
<pre>
- Package Manager for Kubernetes and Openshift
- Using Helm
  - we can install/upgrade/uninstall software into Kubernetes, Rancher and Openshift
- In case the application you are trying to deploy requires multiple Openshift manifest scripts(i.e yaml) files, we
  need to ensure they are applied following a proper sequence, in case we use Helm to package those scripts, Helm is
  Kubernetes/Rancher/Openshift aware tool, hence it will figure out the order in which Openshift resources must created
- The application packaged by Helm is called Helm Chart
</pre>

## Demo - Installing Helm in Linux
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4
chmod 700 get_helm.sh
./get_helm.sh
```



## Info - Persistent Volume (PV)
<pre>
- Persistent Volume is an external disk or shared path
- Persistent Volume can be provisioned by Openshift Administrators either manually(static) or dynamically using storageclass
- PV are created on the cluster scope, which means any application running within Openshift from any project namespace can bind and use it
- Persistent Volume will usually capture the below attributes
  - disk size in MiB/GiB
  - access 
    - ReadWriteOnce ( This means all Pods from a single Openshift node can read and write to the external disk )
    - ReadWriteMany ( This means, all Pods from any node in Openshift can read and write to the external disk )
- this external disk i.e Persistent Volume could be coming from
  - NFS Server
  - AWS S3 bucket
  - AWS EBS - Elastic Block Storage or similar Storage Clusters
  - Longhorn, etc.,
</pre>

## Info - StorageClasss
<pre>
- StorageClass is a way automatically Persistent Volume(PV) can be provisioned in NFS, AWS S3, etc
- For instance, your organization is interested in provisioning Persistent Volumes from AWS S3 buckets, then
  we need to create StorageClass ( via YAML ) in Openshift, providing the AWS Login credentials, any configuration required
- anytime some application in Openshift requests for external storage via Persistent Volume Claim(PVC), then the AWS Storage Class you created
  will automatically provision the request disk with the appropriate disk access and let your application use that storage
</pre>

## Info - Persistent Volume Claim (PVC)
<pre>
- Persistent Volume Claims are crated as part of your application i.e non-admins can create this
- it will be always created under some project namespace
- this is the way your application can request for external storage with specific permission with specific etc.,
</pre>

## Info - ConfigMap
<pre>
- is a Map that stores data in the form key/value
- any non-sensitive data your application depends on can be stored in ConfigMap avoid hard-coding
</pre>

## Info - Secrets
<pre>
- is a Map that stores data in the form of key/value
- all the values stored inside Secrets are just base64 encoded strings, hence if you are using Secrets for security, this is the not right choice
- any sensitive data like login credentials, private/public key, any certs, etc. you may store in Secrets
- only administrators will be able to view the data stored in Secrets, for others they can't see the data/values stored against a key
- Openshift application can securely retrieve data stored from secrets and use them without revealing the values
</pre>

## Lab - Deploying multipod Wordpress and Mariadb in declarative style

In case you haven't clone this repository already, you may do now
```
cd ~
git clone https://github.com/tektutor/openshift-march-2026.git
```

Let's deploy the wordpress and mariadb multi-pod application declaratively using manifest(yaml) files
```
cd Day3/wordpress-with-configmaps-and-secrets
ls -l
```

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/37a1a6e1-3309-4c44-860f-6d55a16b44f6" />


