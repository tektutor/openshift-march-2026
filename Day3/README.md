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
- is an external storage used by applications that needs storage
- is a piece of storage in the Openshift cluster that is provisioned by Openshift Administrator
- it can be either manually provisioned by Openshift Administrator or Dynamically Provisioned
- Persistent Volume can be be provisioned by following backends
  - NFS
  - iSCSI
  - Ceph
  - AWS EBS
  - GlusterFS
  - etc
- PVs are cluster scoped, it is accessible by applications from any Project namespace
- Persistent Volumes usually will have the follow properties
  - Storage Size in Mi, Gi, Ti
  - Access Mode
    - ReadWriteOnce, ReadWriteMany, ReadOnlyMany, ReadOnlyOnce, ReadWriteOncePod
  - StorageClass (optional) - in case provisioned dynamically
  - Labels ( Optional )
</pre>

## Info - Persistent Volume Claim (PVC)
<pre>
- is a request for storage by your application
- any application that needs external storage to peristent their application logs or data will have request by creating PVC
- Project namespace scoped ( tied to a project )
- Key Properties
  - Storage Size in Mi, Gi, Ti, etc.,
  - Access Mode
  - StorageClass (Optional)
  - Labels (Optional)
- If the Storage Controller is able to find an exact match of PV as per your PVC request then it will let your PVC bound and use it in your application deployments
</pre>

## Info - StorageClasss
<pre>
- StorageClass is a way automatically Persistent Volume(PV) can be provisioned in NFS, AWS S3, etc
- For instance, your organization is interested in provisioning Persistent Volumes from AWS S3 buckets, then
  we need to create StorageClass ( via YAML ) in Openshift, providing the AWS Login credentials, any configuration required
- anytime some application in Openshift requests for external storage via Persistent Volume Claim(PVC), then the AWS Storage Class you created
  will automatically provision the request disk with the appropriate disk access and let your application use that storage
</pre>

## Lab - Kindly check to verify NFS shared folder allocated for you
Replace 'jegan' with your name
```
showmount -e | grep jegan
```

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

To update 'jegan' with your name
```
cd ~/openshift-march-2026
git pull
cd Day3/wordpress-with-configmaps-and-secrets
sed -i 's/jegan/nitesh/g' *.yml
```

Deploying wordpress and mariadb
```
cd ~/openshift-march-2026
cd Day3/wordpress-with-configmaps-and-secrets
oc project jegan
./deploy.sh
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/df7161ae-42f1-458c-a85a-2f431ec4badf" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c74da9a7-98ff-4ee8-86f1-7ea90ebb06f8" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/9e2e7b88-9f14-44a0-8f2c-bdadf4e786a5" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/241ac314-6a0e-4670-8027-6f1bcbc0be99" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/87e97bda-3f5c-4cb3-8140-62c61fb60b02" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/74f95a91-28eb-49f0-b2e3-5c936f7d7003" />


## Lab - Creating a Helm chart for our wordpress, mariadb multi-pod application
Make sure you delete your existing openshift project
```
oc delete project jegan
oc new-project jegan
```

```
cd ~/openshift-march-2026
git pull
cd Day3
mkdir -p helm/manifest-scripts
cp wordpress-with-configmaps-and-secrets/*.yml helm/manifest-scripts
cd helm

helm create wordpress
tree wordpress

cd wordpress/templates
rm -rf *
cd ../..

cp manifest-scripts/* wordpress/templates
cd wordpress
echo "" > values.yaml
cd ..
tree wordpress
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/aeccbdf6-3561-4376-bc33-965efc96cc4a" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/a6d5bef8-19f1-4131-aa09-9d225e1432de" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/9867b3ea-1349-4ebd-887b-808d329b7325" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/1ecfae91-0401-48bb-84bf-9550601b89ac" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/23ca87fe-b78b-4dd9-b06d-50828c662732" />


Let's clean up the folders and files created by our previous wordpress deployment
```
showmount -e | grep jegan
cd /var/nfs/jegan/mysql
rm -rf *
cd ../wordpress
rm -rf *
```

Let's create Helm chart(installer package) for wordpress & mariadb multi-pod application
```
cd ~/openshift-march-2026/Day3/helm
helm package wordpress
ls -l
helm list

oc get all

helm install wp-jegan wordpress-0.1.0.tgz

oc get pods
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c44dd582-0c8c-4c9e-8103-c49f0b663398" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b6a35c73-7881-4f89-8bda-a1066b4037bb" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/aefb0997-035b-42e9-9d53-21561a9e67ad" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/8541eb2a-9208-4a7d-8672-b6b82b1799bf" />

## Lab - Deploying application into Openshift using S2I Docker Strategy

Note
<pre>
- When we the S2I(Source to Image), we can just provide application source code along with Dockerfile
- Openshift will clone the GitHub repo
- It will look for Dockerfile, it generates BuildConfig 
- It will create an instance of Build(this will create pod to perform Build) using BuidlConfig
  and its follows the instructions present in the Dockerfile to build your
  application into application executable, then it builds custom image
- the newly build custom image will then get pushed into Openshift Internal Image Registry
- it then deploys your application from the Openshift Internal Image Registry
</pre>

```
oc delete project jegan
oc new-project jegan

oc new-app --name hello-micorservice https://github.com/tektutor/spring-ms.git --strategy=docker
oc expose svc/hello-microservice

oc logs -f bc/hello-microservice
```

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d3f28137-0a5c-485a-8f48-83396f840be3" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/ec4418b5-dad6-4aeb-98f7-76fe49636a60" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/4d9fc777-50ae-47e0-98af-5abde01301e8" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/722e9e2d-0b37-40b7-8c25-38d382523674" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/ed3fd97e-f005-472f-87e2-0f841b59344b" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/94b085ce-5564-498c-aa28-00c4c63ba19f" />

## Lab - Deploying application into Openshift using S2I Source Strategy
```
oc delete project jegan
oc new-project jegan

oc new-app --name hello-micorservice https://github.com/tektutor/spring-ms.git --strategy=source
oc expose svc/hello-microservice

oc logs -f bc/hello-microservice
```
