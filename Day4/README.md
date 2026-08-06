# Day 4

## Info - Cloning the training repository
```
cd ~
git clone https://github.com/tektutor/openshift-aug-2026.git

git config --global user.name "Jeganathan Swaminathan"
git config --global user.email "mail2jegan@gmail.com"
git config --global --list
```

## Info - S2I
<pre>
- Openshift supports deploying application from source code from GitHub or similar version control repositories
- Openshift supports the below types of S2I strategies
  1. Source
  2. Docker
  3. Custom
  4. Binary
  5. Pipeline
- Openshift also supports deploying application from a pre-built container image just like Kubernetes
- In case of Source strategy
  - we need to provide the code repository url
  - we need to mention what container image is suitable to build and run our application
  - Openshift will auto-generate
    - ImageStream
    - BuildConfig
    - Dockerfile
    - Creates
      - Deployment yaml to deploy your application
      - Service to export your application
      - We need to create route ourself
  - Openshift will clone the source code, builds it, prepares a custom container image with your application binary
  - Pushes the Custom Image into Openshift's Internal Container Registry
  - Deploy's your application from the Custom Image it pushed in the Internal Registry
- In case of Docker strategy
  - We need to provide the code repository url that has our application source code
  - We also need to provide Dockerfile in the same code repository
  - Openshift will auto-generate
    - ImageStream
    - BuildConfig
    - Creates
      - Deployment yaml to deploy your application
      - Service to export your application
      - We need to create route ourself
  - Openshift will clone the source code, builds the image using your Dockerfile
  - Pushes the Custom Image into Openshift's Internal Container Registry
  - Deploy's your application from the Custom Image it pushed in the Internal Registry
</pre>

## Lab - Deploying your application using S2I source strategy
```
oc delete project jegan-project
oc new-project jegan-project

oc new-app --name=hello-s2i-source registry.access.redhat.com/ubi8/openjdk-17~https://github.com/tektutor/spring-ms.git --strategy=source
oc expose svc/hello-s2i-source

oc logs -f bc/hello-s2i-source

oc get route

curl http://<your-route-url>
curl http://hello-s2i-source-jegan.apps.ocp4.palmeto.org
```

## Lab - Deploying your application using S2I docker strategy
```
oc delete project jegan-project
oc new-project jegan-project

oc new-app --name=hello-s2i-docker https://github.com/tektutor/spring-ms.git --strategy=docker
oc expose svc/hello-s2i-docker

oc logs -f bc/hello-s2i-docker

oc get route

curl http://<your-route-url>
curl http://hello-s2i-docker-jegan.apps.ocp4.palmeto.org
```

## Info - Node Affinity 
<pre>
- is a way, applications can request for nodes that meets certain criteria
- For instance, let's say there is an application that does loads of disk intensive operations (read/write), it 
  would be preferable to deploy those applications on a node that has SSD storage which is comparatively faster than old style HDD
- In such case, the application can express its criteria under the node affinity section in the yaml file
  - there are 2 types of criteria
    - Preferred
      - scheduler will look for nodes that meets the criteria expressed by application deployment before scheduling
      - in case scheduler is able to find nodes that meets the criteria, it deploys the pods on those nodes that meets the criteria
      - if there are no nodes that meets the criteria, then scheduler will as usual deploy those pods on any node
    - Required
      - scheduler will look for nodes that meets the criteria expressed by application deployment before scheduling
      - in case scheduler is able to find nodes that meets the criteria, it deploys the pods on those nodes that meets thee criteria
      - if there are no nodes that meets the criteria, then scheduler will not deploy those pods on any node until such criteria is met
- Some practical usecases
  - in a openshift cluster that is used by multiples team in an organization, it is possible to label certain nodes as env=dev
    env=qa, env=pre-prod, etc
  - whenever dev team deploys an application they will add the node-affinity section, hence their application will only get deployed
    into nodes that are alloted for the dev team, similarly each team can have their own dedicated nodes reserved for their use
</pre>


## Lab - Node affinity
```
cd ~/openshift-aug-2026
git pull

cd Day4/node-affinity
ls -l

# Remove the label
oc label node/worker02.ocp4.palmeto.org disk-
oc label node/worker03.ocp4.palmeto.org disk-

# First try to list nodes that has a label disk=ssd, you should see no nodes
oc get nodes -l disk=ssd

# Assuming no nodes meets that criteria, let's deploy the preferred node affinity deployment
oc project jegan-project
oc apply -f preferred-node-affinity.yml
# Notice, even though there are no node that has SSD storage, the pods are still deployed
oc get pods -o wide

# Now, try to label worker02 with disk=ssd label
oc label node/worker02.ocp4.palmeto.org disk=ssd
oc get pods -l disk=ssd 
# As the pods are already deployed and they are running, labeling a node with disk=ssd at this point has no impact

# Now, let's delete the preferred nginx deployment
oc delete -f preferred-node-affinity.yml

# As there is a node that meets the criteria
oc get nodes -l disk=ssd
oc apply -f preferred-node-affinity.yml
# We can see, all pods are deployed onto worker2
oc get pods -o wide

# Now, let's delete the preferred nginx deployment
oc delete -f preferred-node-affinity.yml

# Now, let's remove the label from worker02
oc label node/worker02.ocp4.palmeto.org disk-
oc apply -f required-node-affinity.yml
# As there are no nodes that meets the criteria, the pods will not be deployed
oc get pods -o wide
oc describe deploy/nginx

# Now, try to label worker02 with disk=ssd label
oc label node/worker02.ocp4.palmeto.org disk=ssd
oc get nodes -l disk=ssd 
# Now all pods will be deployed into worker 2
oc get pods -o wide

# Once you are done with this exercise, you may delete the deployment
oc delete -f required-node-affinity.yml
```

## Info - LoadBalancer Service
<pre>
- this is an external service, used in managed Kubernetes and/or Openshift clusters running in AWS/Azure or similar public cloud env
- For instance, AWS has ALB ( Application Load Balancer ), Azure has Azure Load Balancer for this they will charge
- when you create a LoadBalancer service on eks, aks, ROSA or ARO, then it creates a Load Balancer on that public cloud
- this type of service is really not meant for local Kubernetes or Openshift clusters
- if at all you wanted to enable this LoadBalancer service on your local Kubernetes and/or Openshift, you need to install
  MetalLB Operator
- Metallb operator will create a loadbalancer within your openshift cluster and let's that load balancer forward the calls to 
  your application pods ( basically load-balances your pods )
- in case of ClusterIP and NodePort Service, kube-proxy performs load-balancing
- in this case, the Metallb load balancer will perform the load balancing for your pods just like AWS ALB or Azure ALB does it
  in eks, aks, ROSA or ARO
</pre>

## Lab - Let's create a load balancer for nginx deployment
```
oc delete project jegan-project
oc new-project jegan-project

oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.26 --replicas=3
oc get deploy,rs,po

# Delete existing nginx service
oc delete svc/nginx

oc expose deploy/nginx --type=LoadBalancer --port=8080
oc get svc
oc describe svc/nginx

curl http://192.168.100.50:8080
```

## Info - Ingress
<pre>
- it is not a service
- it is a set of forwarding rules
- in order to make the Ingress work, your openshift cluster must have a respective Ingress Controller
- depending on which LoadBalancer is installed as part of Openshift setup, the Ingress Controller can be used
- in our lab setup, I have installed HAProxy Load Balancer, hence the type of Ingress Controller will be HAProxy Ingress Controller
- whenever someone creates a Ingress rule in their project namespace, the HAProxy Ingress Controller will detect it
  and it picks those forwarding rules mentioned in the ingress and configures the HAProxy Load Balancer in the openshift cluster
- so there are 3 components involved for the Ingress to work
  1. Ingress ( set of forwarding rules )
  2. HAProxy Ingress Controller
  3. HAProxy Load Balancer
- Ingress will provide a public url
- behind an Ingress, there will a one or more services ( ClusterIP, NodePort, LoadBalancer services )
- behind a service, there will be load of Pods
</pre>


## Lab - Ingress

We need atleast 2 deployment in the project to demonstrate this effectively

Hence, let's deploy nginx in the project
```
oc delete project jegan
oc new-project jegan

oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/bitnami-nginx:1.26 --replicas=3
oc get deploy
oc expose deploy/nginx --port=8080
```

Let's deploy the hello microservice
```
oc project jegan
oc create deploy hello --image=image-registry.openshift-image-registry.svc:5000/openshift/hello:1.0 --replicas=3
oc get deploy
oc expose deploy/hello --port=8080
```

Let's create the ingress
```
cd ~/openshift-aug-2026
git pull
cd Day4/ingress
cat ingress.yml
oc apply -f ingress.yml

oc get ingress
oc describe ingress/tektutor

curl http://tektutor.apps.ocp4.palmeto.org/nginx
curl http://tektutor.apps.ocp4.palmeto.org/hello
```
