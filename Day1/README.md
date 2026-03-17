# Day 1

## Info - Hypervisor Overview
<pre>
- is Virtualization technology
- this helps us run multiple OS on the same Laptop/Desktop/Workstation/Server
- i.e more than one OS can be actively running side by side
- let's we have a laptop with Quad Core CPUs, 8GB RAM and 500GB HDD/SDD, 
  how many maximum Virtual Machines this laptop would support ?
  - the deciding/limiting factor is the Processor supports how many CPU Cores?
  - 4 x 2 = 8 virtual cores you have, which mean total OS that can run on this laptop is limited to 8 ( 1 Host OS +  7 Virtual Machines)
  - with the help of HyperThreading feature, each Physical CPU Core supports running 2 parallel threads, hence they are seen/considered as two virtual/logical Core by the Hypervisor software
  - We could install any Operating System within a Virtual Machine and they are called as Guest OS
- there are 2 types of Hypervisors
  1. Type 1 ( a.k.a Bare-Metal Hypervisors )
  2. Type 2 ( a.k.a Hosted Hypervsiors
- In order to do install any Hypervisor, we need a machine that supports Processor with Virtualization support
  - In case of Intel Processor, the virtualization feature set is called VT-X
  - In case of AMD Processor, the virtualization feature set is called AMD-V
- Type 1
  - is meant to be used in Workstations and Servers
  - this type of Hypervisor comes with a minimal OS, hence we don't need to install Host OS, instead we can directly install the Hypervisor
  - examples
    - VMWare vSphere(vCenter)
    - Microsoft Hyper-V
    - Linux KVM
- Type 2
  - can be installed on top a Host OS ( Linux, Windows or Mac OS )
  - is meant to be used in Laptops, Desktops and Workstations
  - examples
    - VMware Workstation ( supported in Linux and Windows )
    - VMWare Fusion ( supported in Mac OS-X )
    - Parallels ( supports in Mac OS-X )
    - Oracle VirtualBox ( supported in Windows, Linux and Mac OS-X )
- this virtualization technology disrupted the way the IT industry works
- irrespective of Company size, pretty every organization started using this virtualization technology, which resulted in huge
  cost saving
- Server consolidation is possible with Virtualization Technoloyy
- Modern high-end servers could host 2000+ virtual machines in a single server these days
- Though the virtualization technology resulted in huge cost-saving, it has not brought the cost to the extent that every
  engineer can be provided with 10~15 VMs
- Developers would need to test their feature in different OS
- QA team may have test the product on multiple OS 
- In production environment, different software component like Web/App Servers, DB Servers, would require separate OS that runs in a separate VM/Physical Machine
- This type of Virtualization is called Heavy weight Virtualization
  - each VM must be allocated with dedicated Hardware resources
    - CPU cores
    - RAM
    - Storage Hard disk / SSD 
  - each VM, represents one fully functional Operating System
  - each Guest OS that runs on the VM, has its own Network Stack, Network Cards, it has its own OS Kernel, etc.,
</pre>

## Info - Containerization
<pre>
- it is an application virtualization technology
- this is light-weigth virtualization, as container does't required dedicated hardware resources
- in other words, all containers that runs in the same machine/os shares the hardware resources on the underlying Host/Guest OS
- each container represents one application process not an Operating System
- technically, comparing a container with Virtual Machine, Guest OS or Host OS is ideally wrong
- Linux Kernels supports 2 interesting features, which are required by any Container Engine/Runtime
  - Control Groups ( CGroups )
    - this helps applying resource quota restrictions on individual container level
    - for example
      - using CGroups, we can restrict how many CPUs one container can use at the max at point of time
      - using CGroups, we can restrict how much RAM a container can use at the max
      - using CGroups, we can restrict how mauch disk space a container can be utilizat at the max
      - all these helps, one container use up all the storage, cpu or RAM etc, leaving no resources for other containers
  - Namespace
    - this helps isolating one container from the other
- the reason why, people tend to compare an Operating System or Virtual Machine with container technology is for the following reasons
  - each container has its own dedicated Network Stack ( Software defined Network Stack - Virtual )
  - each container has its own dedicated NIC ( Network Card )
  - each container acquires one or more IP Address ( mostly Private IPs ), depending on how many Network Cards it has 
  - every container uses about 7~8 namespaces
- We need to understand two different software related to Container Technology
  1. Container Runtime
  2. Container Engine
</pre>


## Info - Container Runtime
<pre>
- Container Runtimes depend on the Linux Kernel features i.e CGroups and Namespace
- Container Runtimes helps us manage Container Images and Containers
- they are low-level software, hence they are not directly used by end-users like us as they are not so user-friendly
- examples
  - cRun
  - runC
  - CRI-O
</pre>

## Info - Container Engine
<pre>
- They are high-level, user-friendly softwares that helps us manage Container Images and Containers
- Container Engine, internally depends on Container Runtime to manage Container Images and Containers
- examples
  - Docker - depends on Containerd, which in-turn depends on runC Container Runtime
  - Podman - depends on CRI-O container Runtime
  - Containerd - depends on runC Container Runtime
</pre>

## Info - Container Image
<pre>
- is the blueprint of a Container, similar to OS ISO files we download for installing Ubuntu or Windows OS
- using a Container Image, we can create multiple containers
- Container Image names might in some case resemble like an OS, they Container Images don't represent a OS
  - OS Container Image will have mostly the following software tools
    - Package Manager 
      - used to install/uninstall/upgrade softwares
    - comes with minimal linux commands
    - never comes with OS Kernel or full sets of linux commands
- Container Images are conservatively built to reduce the size of image, hence few commands are supported within a container
- Usually Container Image will have
  - One main application with all its dependent libraries and Web/App Servers(if required) to run one single application
</pre>


## Info - Containers
<pre>
- Container is a running instance of a Container Image
- Whatever softwares were pre-installed, pre-configured in the Container Image are available in the Container
- Each container gets it own IP Address
- Each container gets it own File System ( folders and utilities )
- Each container gets it own Port Range ( 0 to 65535 Porst just like an OS )
- General Recommned practive
  - One main application per Container
- examples
  - mysql container
  - weblogic contianer
  - microservice
  - web server
  - app server
  - REST API
  - SOAP API
</pre>

## Info - Docker Registry
<pre>
- Docker Registry is a collection of many Docker Images
- There are 3 types of registries
  1. Local Docker Registry ( /var/lib/docker folder in Linux )
  2. Private Docker Registery ( could be setup using JFrog Artifactory or Sonatype Nexus )
  3. Docker Hub - Remote Registry ( Docker Hub Website )
</pre>

## Info - Docker Overview
<pre>
- Docker is developed in Golang by an organization called Docker Inc
- comes in 2 flavours
  1. Docker Community Edition - Docker CE ( opensource )
  2. Docker Enterprise Edition- Docker EE ( requires license )
     - comes with enterpise registry with access to certified Docker images ( access to Docker certified images )
     - world-wide support from Docker Inc
- it follows Client/Server Architecture
  - docker - is the client tool
  - dockerd - is the Docker Server that runs as a background service 
- In local machines, docker client and server communicates by default using a socket
- In remote machines, docker client and server communicates using REST API
</pre>

## Info - Docker High-Level Architecture
![Docker](DockerHighLevelArchitecture.png)


## Demo - Installing Docker Community Edition ( Docker is already installed in our lab machine - hence you don't need to install it )
```
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker

sudo usermod -aG docker $USER
id
su $USER
id
docker --version
docker info
docker images
```

## Lab - Check if docker is accessible to you`
```
docker --version
docker info
docker images
```

## Lab - Listing the docker images from your Docker Local Registry
```
docker images
```

## Lab - Downloading docker images from Docker Hub Remote Registry to Local Docker Registry
```
docker images

docker pull hello-world:latest
docker pull ubuntu:latest
docker pull nginx:latest

docker images
docker pull mysql:latest
```

## Lab - Create a container and run it in background(as a daemon)
```
docker run -dit --name ubuntu1 --hostname ubuntu1 ubuntu:latest /bin/bash
```

In the above command
<pre>
docker - is the client tool
run - command create the container and starts it
dit - interactive terminal will be started in the background(daemon mode)
name - is the way the docker server will refer the container, to avoid conflicts name your conainer like ubuntu1-jegan
hostname - is the way your OS will connect to the container just like how we access a VM/OS using its hostname or IP Address
         - give the hostname as ubuntu1-jegan ( replace 'jegan' with your name )
ubuntu:latest - is the name of the docker image, latest is the tag(version) of the Docker image
/bin/bash - starts the bash terminal once the container starts running
</pre>

To the list the running containers
```
docker ps
docker ps | grep jegan
```

To list all containers, even if they are not running currently
```
docker ps -a | grep jegan
```

## Lab - Stopping a running a container
```
docker ps
docker stop ubuntu2
docker ps
```

## Lab - Start an exited container
List all containers
```
docker ps -a
```

Start the exited contianer
```
docker start ubutnu2
docker ps
```

## Lab - Deleting a running container gracefully

List the running containers
```
docker ps | grep jegan
```

Stop the container you wish to delete
```
docker stop ubuntu1
```

Delete the container 
```
docker rm ubuntu1 
```

## Lab - Deleting a running container forcibly

List the running containers
```
docker ps | grep jegan
```

Delete the container forcibly
```
docker rm -f ubuntu1 
```


## Lab - Rename a container

In case, you dont' have ubuntu1 and ubuntu2 containers, you may create now or you can skip this step you have them already
```
docker run -dit --name ubuntu1-jegan --hostname ubuntu1-jegan ubuntu:latest /bin/bash
docker run -dit --name ubuntu2-jegan --hostname ubuntu1-jegan ubuntu:latest /bin/bash
```

List the containers
```
docker ps
```

Rename the containers
```
docker rename ubuntu1 c1
docker rename ubuntu2 c2
```

## Lab - Let's create a mysql db server container
```
docker run -d --name mysql-jegan --hostname mysql-jegan -e MYSQL_ROOT_PASSWORD=root@123 mysql:latest
docker ps
```

Get inside the mysql container shell, when it prompts for password you can type root@123
```
docker exec -it mysql-jegan /bin/sh
mysql -u root -p

SHOW DATABASES;
CREATE DATABASE tektutor;
USE tektutor;
SHOW TABLES;

CREATE TABLE trainings ( id INT NOT NULL, name VARCHAR(250) NOT NULL, duration VARCHAR(250) NOT NULL, PRIMARY KEY(id) );
SHOW TABLES;

INSERT INTO trainings VALUES ( 1, "DevOps", "5 Days" );
INSERT INTO trainings VALUES ( 2, "Advanced Openshift", "5 Days" ); 
INSERT INTO trainings VALUES ( 3, "Microserves in Golang", "5 Days" ); 

SELECT * FROM trainings;

exit
exit
```

List the mysql container
```
docker ps -f "name=mysql-jegan"
docker restart mysql-jegan
```

Connect to mysql server, you are supposed to see the tektutor database and training table along with the records intact
```
docker exec -it mysql /bin/sh
mysql -u root -p
SHOW DATABASES;
USE tektutor;
SELECT * FROM trainings;
```

## Lab - Using external storage to store your database, table, record, etc
```
mkdir -p /tmp/jegan/mysql
docker run -d --name mysql-jegan --hostname mysql-jegan -e MYSQL_ROOT_PASSWORD=root@123 -v /tmp/jegan/mysql:/var/lib/mysql mysql:latest
docker ps
docker exec -it mysql-jegan /bin/sh
mysql -u root -p

SHOW DATABASES;
CREATE DATABASE tektutor;
USE tektutor;
SHOW TABLES;

CREATE TABLE trainings ( id INT NOT NULL, name VARCHAR(250) NOT NULL, duration VARCHAR(250) NOT NULL, PRIMARY KEY(id) );
SHOW TABLES;

INSERT INTO trainings VALUES ( 1, "DevOps", "5 Days" );
INSERT INTO trainings VALUES ( 2, "Advanced Openshift", "5 Days" ); 
INSERT INTO trainings VALUES ( 3, "Microserves in Golang", "5 Days" ); 

SELECT * FROM trainings;

exit
exit
docker rm -f mysql-jegan
```

Let's create a new mysql container
```
docker run -d --name mysql-jegan1 --hostname mysql-jegan1 -e MYSQL_ROOT_PASSWORD=root@123 -v /tmp/jegan/mysql:/var/lib/mysql mysql:latest
docker ps
docker exec -it mysql1-jegan /bin/sh
mysql -u root -p

SHOW DATABASES;
CREATE DATABASE tektutor;
USE tektutor;
SHOW TABLES;
SELECT * from trainings;
exit
exit
```

Troubleshooting Mysql Innodb crash issue ( I have already fixed it - this is just for your reference )
The linux kernel parameter restricts how many Asynchronous IO operations can happen in parallel. Increasing this limits solves the issue.
```
cat /proc/sys/fs/aio-max-nr
sysctl -w fs.aio-max-nr=262144
```

## Info - Every container 
<pre>
- every containers uses seven namespaces
  1. PID Namespace
  2. Network Namespace
  3. Mount Namespace
  4. Unix Timesharing System (UTS)
  5. IPC Namespace
  6. User Namespace
  7. CGroup Namespace
</pre>  

## Lab - Setup a LoadBalancer with 3 backend Webservers containers ( Port Forwarding )

<img width="2968" height="1960" alt="image" src="https://github.com/user-attachments/assets/be769574-258f-49fc-b212-ced75e7504ae" />


Let's create 3 web server containers using nginx:latest docker image
```
docker run -d --name nginx1-jegan --hostname nginx1-jegan nginx:latest
docker run -d --name nginx2-jegan --hostname nginx2-jegan nginx:latest
docker run -d --name nginx3-jegan --hostname nginx3-jegan nginx:latest
```

Let's create a Load Balancer container with port-forwarding to expose it for external access
```
docker run -d --name nginx-lb-jegan --hostname nginx-lb-jegan -p 8001:80 nginx:latest
```

Let's list all these containers
```
docker ps -f "name=jegan"
docker ps | grep jegan
```

Find the IP Address of your nginx1-jegan, nginx2-jegan and nginx3-jegan containers and replace in the nginx.confi file below
```
docker inspect nginx1-jegan | grep IPA
docker inspect nginx2-jegan | grep IPA
docker inspect nginx3-jegan | grep IPA
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/e39bc9a9-8e05-4ade-b22d-98ee976cd794" />

Now, we need to configure the nginx-lb-jegan container as a Load Balancer( by default it works as Web Server ).
In order to configure, let's copy the nginx.conf file from the nginx-lb-jegan container to our local machine.
```
docker cp nginx-lb-jegan:/etc/nginx/nginx.conf .
```

Edit the copied nginx.conf file as shown below
```
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    upstream backend {
        server 172.17.0.8:80;
        server 172.17.0.9:80;
        server 172.17.0.10:80;
    }

    server {
        location / {
            proxy_pass http://backend;
        }
    }
}
```

Let's copy this nginx.conf file back to the lb container
```
docker cp nginx.conf nginx-lb-jegan:/etc/nginx/nginx.conf
```

In order to force apply the config changes, let's restart the nginx lb container
```
docker restart nginx-lb-container
```

Check if your lb container is running after your config changes
```
docker ps | grep nginx-lb-jegan
```


Let's customize the index.html pages in nginx1-jegan, nginx2-jegan and nginx3-jegan
```
echo "Server 1" > index.html
docker cp index.html nginx1-jegan:/usr/share/nginx/html

echo "Server 2" > index.html
docker cp index.html nginx2-jegan:/usr/share/nginx/html

echo "Server 3" > index.html
docker cp index.html nginx3-jegan:/usr/share/nginx/html
```

Test your LB configuration
```
curl http://192.168.0.200:8001
curl http://192.168.0.200:8001
curl http://192.168.0.200:8001
```
<img width="480" height="300" alt="image" src="https://github.com/user-attachments/assets/c5b72cd2-d9d2-482f-b0fe-668800931250" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/477816d2-4c7b-469f-9d81-58ecc4c60b59" />

