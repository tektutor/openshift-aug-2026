# Day 3

## Lab - Deploying application into Openshift using declarative style
```
oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.26 --replicas=3 --dry-run=client -o yaml

oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.26 --replicas=3 --dry-run=client -o yaml > nginx-deploy.yml

cat nginx-deploy.yml

oc delete project jegan-project

oc new-project jegan-project
oc create -f nginx-deploy.yml --save-config=true

oc get deploy,rs,po
```
