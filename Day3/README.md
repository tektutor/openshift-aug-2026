# Day 3

## Lab - Deploying application into Openshift using declarative style
```
# The command below will show the resources that will be created by the below command without actually running it on cluster
# It shows the output of deployment in yaml format
oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.26 --replicas=3 --dry-run=client -o yaml

# This will redirect the yaml output into nginx-deploy.yml
oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.26 --replicas=3 --dry-run=client -o yaml > nginx-deploy.yml

cat nginx-deploy.yml

oc delete project jegan-project

oc new-project jegan-project
oc create -f nginx-deploy.yml --save-config=true

oc get deploy,rs,po
```
