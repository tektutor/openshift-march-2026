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
oc new-project jegan
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

## Lab - Setup Jenkins CI CD Pipeline in Openshift
```
cd ~
git clone https://github.com/tektutor/openshift-march-2026.git
cd openshift-march-2026/Day5/CICD
ls -l
oc project jegan
oc create imagestream hello-microservice
oc create secret generic generic-webhook-secret --from-literal=WebHookSecretKey=trainee-lab-123
oc apply -f buildconfig.yml
oc policy add-role-to-user edit system:serviceaccount:jegan:default
oc start-build java-app-pipeline --follow
```
<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/eea7a6f3-caad-4e56-a1bc-f2d29dd48b4c" />
<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/0bb4038d-7361-41d3-b5c7-f89dbf6087de" />
<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/3678d4c7-dba9-42b4-97ed-b46737c21af4" />
<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/8a0fa0bd-78e0-488b-aca5-363a328e484c" />
<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/22f192fc-786e-4007-83dd-5c6deca9f0a4" />
<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/2a07b221-42d7-4973-a8a8-d046d3b56a68" />
<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/b40bc751-0884-4434-8094-288a59fcd079" />
<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/30e36489-889c-41e0-abc4-e6b1805bd276" />
<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/fa6f0be6-741b-485a-8a18-79b6f719acb7" />
<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/80052043-63fd-45d1-945a-e0e2cdf385fe" />
