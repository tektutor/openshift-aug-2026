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
oc apply -f nginx-deploy.yml

oc get pods

# Scale down
# Update the nginx-deploy.yml, update replicas from 5 to 3, save and exit
oc apply -f nginx-deploy.yml

oc get pods

```
