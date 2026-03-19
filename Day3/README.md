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
<img width="480" height="300" alt="image" src="https://github.com/user-attachments/assets/ca4fe105-d09b-4974-b9b2-3406bdb656bb" />

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
