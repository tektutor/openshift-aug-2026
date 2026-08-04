# Day 1

## Info - Bootloader - Multibooting/Dual Booting
<pre>
- Boot loader is a tiny system utility that gets installed in Boot sector of your hard disk
- When the system is booted, the BIOS POST (Power On Self Test completes) and BIOS firmware
  instructs the CPU to run the bootloader at Sector 0, Byte 0 (Boot Sector )
- Once the CPU starts the boot loader system utility, it will scan your system looking for hard disks ( SSD ),
  detecting for all the Operating Systems installed on your machine
- In case, it detects more than one OS, it presents a menu for you to choose between the OS
- Only one OS can be active at any point of time
- Let's say, your laptop has got Windows 11 and Ubuntu 24.04, and you booted your laptop into Windows 11, in order
  to use Ubuntu 24.04, you need to shutdown the Windows 11 OS and boot in Ubuntu 24.04 and vice versa
- examples
  - Boot Camp ( commercial product used in Macbooks to install Windows )
  - LILO 
  - GRUB ( all latest Linux distributions use this )
</pre>
  
## Info - Hypervisor Overview
<pre>
- it is virtualization technology
- with virtualization, we can run multiple OS on the same machine side by side
- i.e more than one OS can actively run in the same laptop/desktop/workstation/server
- there are 2 types of Hypervisor
  1. Type 1 - a.k.a Bare Metal Hypervisor
     - used in Servers & Workstations
     - examples
       - VMWare vSphere(v-center), Linux KVM, Microsoft Hyper-v, zen, etc.,
  2. Type 2 - a.k.a Hosted Hypervisor
     - used in Laptops/Desktops/Workstation where already there is Host OS pre-installed(Windows, Mac OS-X or Linux )
     - examples
       - VMWare Workstation, VMware Fusion, Oracle VirtualBox, Parallel, etc.,
- each VM represents one Operating System with its own OS Kernel
</pre>

## Info - High Level Hypervisor Architecture
![hypervisor](HypervisorHighLevelArchitecture.png)

## Info - Containerization
<pre>
- is an application virtualization technology
- it is light-weight technology as all containers running on the same Host OS or Guest OS shares the Hardware
  resources on the underlying Host/Guest OS
- container represents an application not an OS
- container doesn't have its own OS Kernel, hence it depends on Host/Guest OS Kernel
- in some ways container and virtual machines behave similar
  - just like a VM has its own virtual network card(s), containers also has its own virtual network card(s)
  - just like a VM gets an IP address, containers also get its own IP address
  - just like a VM has its own software defined network stack, containers also has its own software defined network stack
  - network stack ( 7 OSI Layers )
  - Just like VMs, containers also has a file system ( files & folders )
- but technically comparing a Virtual Machine Guest OS with container is wrong, as Guest OS is a fully functional OS
  with its own Kernel, while container is just an application not a OS, it doesn't have its own OS Kernel
- one container represents one application
- containers will never able to replace OS or Virtualization
- in real world, containers runs inside VMs, VMs runs inside Physical Servers
- Containerization depends on 2 Linux Kernel features
  1. Namespace and
     - is used to isolate one container from the other
  2. Control Groups ( CGroups )
     - is used to apply resource quota restricts like
     - how many CPUs a container can use at the max
     - how much RAM/disk a container can use at the max
</pre>

## Info - High Level Docker Architecture
![docker](DockerHighLevelArchitecture.png)

## Info - Container Engine
<pre>
- Container Engine is a high-level software, that manages container images and containers
- Under the hood, Container Engine depends on Container Runtimes
- Container Engine is user-friendly, it nicely abstract the linux kernel low-level stuffs and provides an easier
  interface to manages images and containers
- examples
  - Docker
    - depends on containerd, which in turn depends on runC container Runtime
  - Podman
    - depends on CRI-O container runtime
</pre>

## Info - Container Runtime
<pre>
- Container Runtime is a low-level software, that manages container images and containers
- they are not so user-friendly, hence end-user almost never use container runtimes
- example
  - runC, cRun, CRI-O, rkt, etc.,
- depends on the Linux Kernel Namespace & CGroups
</pre>

## Info - Container Image
<pre>
- is a blueprint/template of a container
- using Container Image, we can create as many containers as we need
- packages a single application with all its dependent libraries, runtimes, etc.,
- it has unique name and ID
- the ID is a 256-bit Hash
</pre>


## Info - Container
<pre>
- is a running instance of a Container Image
- it has an unique name and ID
- the ID is a 256-bit Hash
- one container represents one application
- in some cases, multiple containers would be required to run a single application
- every container runs in a separate namespace
</pre>


## Info - Container Registry
<pre>
- is a collection of one or more Container Images
- Docker supports 3 types of Registries
  1. Docker Local Registry 
     - is a folder on which all container images are cached ( /var/lib/docker is the folder where local registry is maintained)
  2. Docker Private Registry
     - it is a Server
     - it could be Sonatype Nexus or JFrog Artifactory
  3. Docker Remote Registry ( Docker Hub website )
     - is a website maintained by Docker Inc organization along with opensource community support
</pre>


## Info - Docker Overview
<pre>
- Docker is implemented in Go language by an organization called Docker Inc
- Docker comes in 2 flavours
  1. Docker Community Edition - Docker CE ( opensource )
  2. Docker Enterprise Edition- Docker EE ( licensed product )
</pre>

## Info - Installing Docker in Ubuntu
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
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update

sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
newgrp docker
docker --version
docker images
```

## Lab - Checking your docker version and installation details
```
docker --version
docker info
```

<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/b848ed2e-3553-474f-b7b8-288ac55d096e" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/6c2ba3fe-72aa-479a-9f6f-31ad0d77b13b" />


## Lab - Listing docker images from your local docker registry
```
id
docker images
# Troubleshooting Permission denied error
newgrp docker
id

docker images
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/fac60bb4-a5ae-4a7d-b2f2-c858f25e4fba" />

## Lab - Download docker image from Docker Hub to your local docker registry
```
docker pull nginx:latest
docker pull mysql:latest
```

<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/647cd5a4-8f99-4b87-b1f9-894725e5ef44" />


List the images from your local docker registry
```
docker images | grep nginx
docker images | grep mysql
```

<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/e384e987-5d8b-4e83-b2e4-16e4d739cad7" />


## Lab - Deleting a docker image from local docker registry
```
# Download hello-world:latest image
docker pull hello-world:latest

# List hello world image
docker images | grep hello-world

# Delete hello world image
docker rmi hello-world:latest

# List hello world image
docker images | grep hello-world
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/e0466cb8-e912-4da6-9db3-3cc26ba29659" />

## Lab - Creating and running the container in the background(daemon/deattached) mode
```
# Try to navigate to /var/lib/docker folder
whoami
cd /var/lib/docker  # You are supposed to get permission denied error as you are not an administrator

# The command creates a new container named ubuntu1-jegan and starts it
docker run -dit --name ubuntu1-jegan --hostname ubuntu1-jegan ubuntu:latest /bin/bash

# List all running containers
docker ps

# Get inside the container shell of ubuntu1-jegan
docker exec -it ubuntu1-jegan /bin/bash
hostname
hostname -i
ls
whoami

# We are coming out of the container shell
exit
```

<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/ab390e7a-de13-483b-a89d-bff83c94820c" />

## Lab - Stopping and Starting your container
List and see your container
```
docker ps | grep jegan
```

Stop your container
```
docker stop ubuntu1-jegan
```

List and see your container
```
docker ps -a| grep jegan
```

Start your container
```
docker start ubuntu1-jegan
```

List and see your container
```
docker ps | grep jegan
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/d5d4ec0c-d278-4098-a9f4-039ed7af2599" />

## Lab - Restarting your container
```
docker ps | grep ubuntu1-jegan
docker restart ubuntu1-jegan
docker ps | grep ubuntu1-jegan
```

<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/f8d8c24b-9548-4f5c-b05b-ecbab7982700" />

## Lab - Delete a container gracefully

In order to delete a container gracefully, you must stop it first
```
docker stop ubuntu1-jegan
```

Then delete it
```
docker rm ubuntu1-jegan
docker ps -a
```

## Lab - Delete a container forcibly
```
docker rm -f ubuntu1-jegan
docker ps -a
```

## Lab - Finding more details about a docker image
```
docker images | grep mysql
docker image inspect mysql:latest
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/792b510e-ceba-41d7-8ab0-65ee7aed3bff" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/00e3c1b7-a66a-4d3e-ae89-5acb098fc2ad" />


## Lab - Create a mysql db server container

Create a mysql db server container, when it prompts for password type root@123 
```
docker run -d --name mysql-jegan --hostname mysql-jegan -e MYSQL_ROOT_PASSWORD=root@123 mysql:latest
docker ps
docker logs mysql-jegan
docker exec -it mysql-jegan /bin/sh
mysql -u root -p

SHOW DATABASES;
CREATE DATABASE tektutor;
USE tektutor;
CREATE TABLE trainings ( id INT NOT NULL, name VARCHAR(250) NOT NULL, duration VARCHAR(250) NOT NULL, PRIMARY KEY(id) );

INSERT INTO trainings VALUES ( 1, "DevOps", "5 Days" );
INSERT INTO trainings VALUES ( 2, "Linux Device Driver", "5 Days" );
INSERT INTO trainings VALUES ( 3, "Microservices in Golang", "5 Days" );

SELECT * FROM trainings;

# Come out of mysql client 
exit

# come out of mysql-jegan container shell
exit
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/21b78ba5-4c86-411c-b412-981647050dc2" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/6f8690b9-525a-456d-ae4d-5c5d08bbb0d5" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/dc9d3b7a-f574-43de-9a1a-2f718231cf00" />

Let's delete the mysql container ( note, deleting container also deletes the tektutor database along with training tables and all its records )
```
docker rm -f mysql-jegan
```

Let's create mysql and let it store the data in an external storage ( this is the recommended practice )
```
mkdir -p /tmp/mysql-jegan
docker run -d --name mysql-jegan --hostname mysql-jegan -e MYSQL_ROOT_PASSWORD=root@123 -v /tmp/mysql-jegan:/var/lib/mysql mysql:latest
docker ps

docker exec -it mysql-jegan /bin/sh
mysql -u root -p

SHOW DATABASES;
CREATE DATABASE tektutor;
USE tektutor;
CREATE TABLE trainings ( id INT NOT NULL, name VARCHAR(250) NOT NULL, duration VARCHAR(250) NOT NULL, PRIMARY KEY(id) );

INSERT INTO trainings VALUES ( 1, "DevOps", "5 Days" );
INSERT INTO trainings VALUES ( 2, "Linux Device Driver", "5 Days" );
INSERT INTO trainings VALUES ( 3, "Microservices in Golan", "5 Days" );

SELECT * FROM trainings;

# Come out of mysql client 
exit

# come out of mysql-jegan container shell
exit
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/a9a90823-0699-43c1-8083-bf9134f3c1bf" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/2ca4f391-e012-40dd-b37c-0335c90d92eb" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/af2d22ac-9f4a-4f36-8938-5bd97c0baa8b" />

Let's delete the mysql container ( note, deleting container will not delete tektutor database or the training tables or any of its records )
```
docker rm -f mysql-jegan
```

Let's create a different mysql container using the same host path mounted inside mysql container
```
docker run -d --name mysql-jegan --hostname mysql-jegan -e MYSQL_ROOT_PASSWORD=root@123 -v /tmp/mysql-jegan:/var/lib/mysql mysql:latest
docker ps

docker exec -it mysql-jegan /bin/sh
mysql -u root -p

SHOW DATABASES; # You are supposed to see tektutor database
USE tektutor;
SHOW TABLES; # You are supposed to see trainings table

SELECT * FROM trainings;  # You are supposed to see all the 3 records that were inserted from the previous container
exit
exit
```

Troubleshooting the mysql container not after 3 instances
```
docker rm -f mysql-jegan
docker run -d --name mysql-jegan --hostname mysql-jegan -e MYSQL_ROOT_PASSWORD=root@123 -v /home/palmeto/mysql-jegan:/var/lib/mysql mysql:latest --innodb-use-native-aio=0

```

## Lab - Finding IP address of a container
```
docker inspect ubuntu1-jegan | grep IPA
docker inspect -f "{{.NetworkSettings.Networks.bridge.IPAddress}}" ubuntu1-jegan
```

## Lab - Checking logs
```
docker run -d --name nginx-jegan --hostname nginx-jegan nginx:latest
docker logs nginx-jegan
```

## Lab - Copying files from local machine to container and vice versa
```
docker inspect nginx-jegan | grep IPA
curl http://172.17.0.2:80

# Copy the index.html from container to local machine
docker cp nginx-jegan:/usr/share/nginx/html/index.html .

# Update the index.html on your local machine
echo "Nginx works!" > index.html

# Copy the index.html from local machine to the container
docker cp index.html nginx-jegan:/usr/share/nginx/html/index.html

curl http://172.17.0.2:80
```

## Lab - Port-forward to expose containerized application to enable external access ( make it accessible outside that machine )
```
docker run -d --name nginx-jegan --hostname nginx-jegan -p 8080:80 nginx:latest
docker ps
```

Accessing the web page
```
curl http://localhost:8080
curl http://192.168.1.199:8080 # In case you are working on server 1
curl http://192.168.1.201:8080 # In case you are working on server 2
```

## Lab - Renaming a container
```
# Notice, I'm given a wrong name for the container below
docker run -d --name nginx-jeg --hostname nginx-jegan nginx:latest
docker ps

# I would like to change the name of the container
docker rename nginx-jegan nginx-jegan
docker ps
```
