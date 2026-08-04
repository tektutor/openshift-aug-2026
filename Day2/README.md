# Day 2

## Info - Container Orchestration Platform Overview
<pre>
- offers in-built features to make your applications high-available (HA)
- it has monitoring features to check whether your application is running, live and ready to server users
- scale up/down is possible when the traffic your application increases/drops down
- rolling update
  - upgrading/downgrading your application version from one to other without any downtime
  - rolling back in case the recently deployed application version is found to be unstable
- in built features to 
  - expose your application for internal access or for external access using services
- supports service discovery
  - accessing an application using its service name
- CI/CD is possible in some container orchestration platforms
- it supports both legacy monolithic applications as well as micro service based applications
- examples
  - Docker SWARM ( opensource )
  - Google Kubernetes ( production grade and opensource )
  - Red Hat Openshift ( it is Red Hat's distribution of Kubernetes )
  - Rancher
  - AWS eks ( AWS Managed Kubernetes cluster )
  - Azure aks ( Azure Managed Kubernetes cluster )
  - AWS ROSA ( AWS Managed Red Hat Openshift cluster )
  - Azure ARO ( Azure Managed Red Hat Openshift cluster )
</pre>

## Info - Kubernetes
<pre>
- it is opensource
- it is command-line tool, there is no production grade webconsole/dashboard
- it is robust and production grade
- it supports many different types of Container Engine and Container Runtimes
- it works as a cluster of many nodes 
- node could be a
  - physical server with some linux os installed on it
  - virtual machine with some linux os installed on it
  - it can be ec2 instance or azure vm with some linux os install on it
  - there are 2 types of nodes
    - master node and
      - Control Plane components will be running
    - worker node
      - this where user application will be running
- it has provides all the basic features required to extend Kubernetes API and/or features
  - Custom Resource Definition and Custom Controller, with this we can any new features and extend Kubernetes
  - Operator => is a combination of many custom resources and custom controller
</pre>

## Info - Red Hat Openshift Overview
<pre>
- it is Red Hat's distribution of Kubernetes 
- it is developed on top of Google opensource Kubernetes
- it is a superset of Kubernetes with many additional useful features
- using Kubernetes Operator, Openshift team has added many additional features
- Openshift 4.x onwards - only supports CRI-O Container Runtime and Podman Container Engine
- Openshift 4.x onwards - the OS that can be installed in master nodes is Red Hat Enterprise Core OS (RHCOS)
- Openshift 4.x onwards - the OS that can be installed in worker nodes is Red Hat Enterprise Core OS (RHCOS) or RHEL
- Red Hat Enterprise Core OS
  - there is highly secured minimal linux operating system
  - this comes with specific version of Podman and CRI-O container runtime pre-installed
  - this works like immutable read-only Operating System with restricted write access
  - this will enforce many best practices
  - certain folders like /bin, /usr, /var, /etc are considered read-only folder, applications
    won't be allowed to modify them, when applications are found to attempt modifying those folders, they won't be
    allowed to run
  - ports below and upto 1024 are reserved for internal use in case of Openshift
- built-in User management is supported (Role based access control supported out of the box )
- built-in Internal Container Registry comes of the box
- New features
  - Route
  - DeploymentConfig
  - Project
  - S2I ( Source to Image )
    - applications can be deployed from source code from version control like Github, bitbucket, etc.,
    - support different strategies
  - BuildConfig, Build
- comes with world-wide support from Red Hat ( an IBM company )
</pre>

## Info - Kubernetes High Level Architecture
![kubernetes](KubernetesArchitecture2.png)

## Info - Red Hat Openshift High Level Architecture
![openshift](openshiftArchitecture.png)
![openshift](master-node.png)
