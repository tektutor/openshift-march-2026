# Day 5

## Info - Openshift Images
<pre>
image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.27
image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.28
image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.29
image-registry.openshift-image-registry.svc:5000/openshift/wordpress:latest
image-registry.openshift-image-registry.svc:5000/openshift/mariadb:12.0.2
</pre>

## Lab - Deploy Jenkins in Openshift

Make sure your existing project is deleted
```
oc delete project jegan
```

Let's create a new project and deploy Jenkins within Openshift
```
oc new-project jegn
oc new-app jenkins-ephemeral
oc status
oc get pods
```
<img width="1913" height="1116" alt="image" src="https://github.com/user-attachments/assets/0846a963-bc04-48d9-aedd-99c0419b40b8" />
<img width="1913" height="1116" alt="image" src="https://github.com/user-attachments/assets/2d06bdea-94f2-4fac-8d81-90004aa4ebe5" />
<img width="1913" height="1116" alt="image" src="https://github.com/user-attachments/assets/b26951a9-9cf7-4c4f-824e-1e3790746ec0" />
<img width="1913" height="1116" alt="image" src="https://github.com/user-attachments/assets/34a8e77e-39a0-4974-a213-2d6e97e894fe" />
<img width="1913" height="1116" alt="image" src="https://github.com/user-attachments/assets/346a1dbb-a6c8-48f0-9adf-180d78458d03" />
<img width="1913" height="1116" alt="image" src="https://github.com/user-attachments/assets/e6db285f-c4ff-4691-9ca6-d08f5d9a552e" />
<img width="1913" height="1116" alt="image" src="https://github.com/user-attachments/assets/f1e97258-2118-434e-8928-1a3e5c1394f1" />
<img width="1913" height="1116" alt="image" src="https://github.com/user-attachments/assets/0df1e8fe-d162-4dc5-8640-29e7211eff79" />
<img width="1913" height="1116" alt="image" src="https://github.com/user-attachments/assets/b2632f27-870c-40ac-b2ed-c7ed46b0da9c" />
<img width="1913" height="1116" alt="image" src="https://github.com/user-attachments/assets/82abb3b9-59c2-4a88-86a8-ec30896c776b" />
<img width="1913" height="1116" alt="image" src="https://github.com/user-attachments/assets/9b07e2a1-aa4a-495d-8cdd-68913c31b901" />
<img width="1913" height="1116" alt="image" src="https://github.com/user-attachments/assets/6296770b-023f-4004-88ab-8c9a8e728f1c" />
