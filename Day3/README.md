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
oc desecribe svc/nginx
```
