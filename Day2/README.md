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
  - AWS eks ( AWS Managed Kubernetes cluster )
  - Azure aks ( Azure Managed Kubernetes cluster )
  - AWS ROSA ( AWS Managed Red Hat Openshift cluster )
  - Azure ARO ( Azure Managed Red Hat Openshift cluster )
</pre>
