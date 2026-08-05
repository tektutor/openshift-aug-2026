# Day 3

## Lab - Deploying application into Openshift using declarative style
```
# The command below will show the resources that will be created by the below command without actually
# running it on cluster. It shows the output of deployment in yaml format
oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.26 --replicas=3 --dry-run=client -o yaml

# This will redirect the yaml output into nginx-deploy.yml
oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.26 --replicas=3 --dry-run=client -o yaml > nginx-deploy.yml

cat nginx-deploy.yml

oc delete project jegan-project

oc new-project jegan-project
oc create -f nginx-deploy.yml --save-config=true

oc get deploy,rs,po
```

## Lab - Creating a ClusterIP Internal service for nginx deployment in declarative style
```
oc project jegan-project

oc get deploy

oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml
oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml > nginx-clusterip-svc.yml

oc create -f nginx-clusterip-svc.yml --save-config=true

oc get services
oc describe svc/nginx
```

## Lab - Creating a NodePort external service for nginx deployment in declarative style
```
oc project jegan-project

oc expose deploy/nginx --type=NodePort --port=8080 --dry-run=client -o yaml > nginx-nodeport-svc.yml
cat nginx-nodeport-svc.yml

# Delete the existing internal service
oc delete -f nginx-clusterip-svc.yml

# Now create the nodeport service
oc apply -f nginx-nodeport-svc.yml

oc get services
oc describe svc/nginx

# You need to find the nodeport allocated for your nginx service and replace the 32200 port with your nodeport
curl http://master03.ocp4.palmeto.org:32200
```

## Lab - Scale up/down a deployment using declarative approach
```
oc project jegan-project

oc get pods

# Scale up
# Update the nginx-deploy.yml, update replicas from 3 to 5, save and exit
sed -i 's/replicas: 3/replicas: 5/' nginx-deploy.yml
oc apply -f nginx-deploy.yml

oc get pods

# Scale down
# Update the nginx-deploy.yml, update replicas from 5 to 3, save and exit
sed -i 's/replicas: 5/replicas: 3/' nginx-deploy.yml

oc apply -f nginx-deploy.yml

oc get pods
```

## Lab - Rolling update - upgrade nginx image version from 1.26 to 1.27
```
oc project jegan-project
oc get pods -o yaml | grep image

# Update nginx image version from 1.26 to 1.27
sed -i 's/1\.26/1\.27/g' nginx-deploy.yml

oc apply -f nginx-deploy.yml
oc rollout status deploy/nginx
oc rollout history deploy/nginx
oc get pods -o yaml | grep image
```

## Info - What happens internally in Openshift when we deploy an application either declaratively or imperative 
Let's say we run the below command, 
```
oc create deploy nginx --image=docker.io/bitnamilegacy/nginx:1.29.1 --replicas=3
```

![internals](openshift-internals.png)

## Lab - Deploying wordpress and mysql multi-pod application in declarative style
```
cd  ~

git clone https://github.com/tektutor/openshift-aug-2026.git
cd openshift-aug-2026
cd Day3/wordpress-with-configmaps-and-secrets

# Make sure all yaml you have replace 'jegan' with your name
# Make sure the NFS server ip is updated before proceeding
# Update the mysql-pv.yml mysql-pvc.yml mysql-deploy.yml, wordpress-pv.yml wordpress.pyc.yml wordpress-deploy.yml
# To find the nfs path reserved for your wordpress and mysql run this command showmount -e | grep jegan

grep -rn jegan *.yml
sed -i 's/jegan/your-name-goes-here/gI' *.yml
./deploy.sh

oc get pv,pvc
oc get pods

oc get route
# Route url you can paste in the lab machine browse and access the wordpress blog page
```


## Info - ReplicationController vs DeploymentConfig vs Deployment vs ReplicaSet
<pre>
- Openshift is running on top of Kubernetes cluster
- In older version of Kubernetes, we need to deploy stateless applications as ReplicationController
- The ReplicationController supports
  - Rolling update and
  - Scale up/down
- As per SOLID Design Principles
  - S - Single Responsibility Principle (SRP)
- each controller is supposed to manage one resource with one functionality
- ReplicationController is responsible for Rolling update and Scaling up/down, which kind
  of violates the Single Responsibility Principle
- ReplicationController doesn't support declarative style of rolling update and scale up/down
- As the ReplicationController doesn't support declarative style, Openshift team introduced DeploymentConfig which
  under the hood uses ReplicationController
- DeploymentConfig supports declarative style
- Meanwhile, Kubernetes team, wanted to refactor ReplicationController
  - ReplicationController was split into two 2
    1. Deployment
      - Deployment Controller is responsible for Rolling update
    2. ReplicaSet
      - ReplicaSet Controller is responsible for Scale up/down
  - In latest Kubernetes, they deprecated use of ReplicationController
    - In the place of ReplicationController for all new stateless application deployments we need
      to use Deployment & ReplicaSet
  - The ripple effect, due to this deprecation, Openshift team deprecated use of DeploymentConfig and
    started encouraging use of Deployment & Replicaset for stateless application
  - As we know Deployment supports declarative approach
- For backward compatiblitity reasons, Kubernetes and Openshift retains ReplicationController
- For the same reason, Openshift retains DeploymentConfig for backward compatability otherwise it is deprecated
</pre>
