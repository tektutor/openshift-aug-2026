# Day 5

## Info - How many users one Pod can handle?
<pre>
- there is no fixed number
- it depends on your application's per-request cost and its concurrency model
- how to compute the concurrent cacacity
  concurrent requests = ( throughput in req/sec ) x ( average request duration in sec )
- if each request takes 200ms and one pod can support upto 250~500 request/second
- a lightweight SpringBoot application with no database dependency with 500m CPU and 512Mi RAM can approximately
  handle 1000+ requests/second
- the same springboot pod with 40ms database query per request drops to roughly 200~400 concurrent users
- Tomcat can handle 10 concurrent database-bound requests
- Benchmarking is the right way to find the number as concurrency depends on your application's ability
  and how long it takes to respond a single request
</pre>  

## Lab - JMS Producer and JMS Consumer microservices communication via AMQ
```
oc delete project jegan-project

oc new-project jegan-project

oc new-app --name=jms-producer https://github.com/tektutor/openshift-aug-2026.git --context-dir=Day5/jms-demo/producer --strategy=docker
oc new-app --name=jms-consumer https://github.com/tektutor/openshift-aug-2026.git --context-dir=Day5/jms-demo/consumer --strategy=docker

oc get pods

# Terminal window 1
oc logs -f jms-producer-5f78665c77-8pchq

# Terminal window 2
oc logs -f jms-consumer-695fdfd78c-gcrlg
```

## Lab - Job
```
cd ~/openshift-aug-2026
git pull
cd Day5/job
oc project jegan-project

oc apply -f job.yml
oc get jobs -f
```

## Lab - CronJob
```
cd ~/openshift-aug-2026
git pull
cd Day5/cron-job
oc project jegan-project

oc apply -f cron-job.yml
oc get jobs -f
```

## Lab - Daemonset
```
cd ~/openshift-aug-2026
git pull
cd Day5/daemonset
oc apply -f hello-daemonset.yml

# You will exactly one pod running on each node as we have 6 nodes in total in our cluster
oc get pods -o wide
```

## Lab - Horizontal Pod Auto-scaling based on CPU Utilization
```
oc delete project jegan
oc new-project jegan

cd ~
git clone https://github.com/tektutor/openshift-aug-2026.git
cd openshift-aug-2026
cd Day5/auto-scaling
oc create -f hello-deploy.yml --save-config=true
oc get pods
oc create -f hello-hpa.yml --save-config

oc expose deploy/nginx --port=8080
oc expose svc/nginx
oc get route
```

We need to stree the pod with more traffic
```
ab -k -n 200000 -c 1000 https://nginx-jegan.apps.ocp4.palmeto.org/
```


## Lab - CICD with Jenkins & Red Hat Openshift
![jenkins](img1.png)
![jenkins](img2.png)
![jenkins](img3.png)
![jenkins](img4.png)
![jenkins](img5.png)
![jenkins](img6.png)
![jenkins](img7.png)
![jenkins](img8.png)
![jenkins](img9.png)
![jenkins](img10.png)
![jenkins](img11.png)
![jenkins](img12.png)
![jenkins](img13.png)
![jenkins](img14.png)
![jenkins](img15.png)
![jenkins](img16.png)
![jenkins](img17.png)
![jenkins](img18.png)
![jenkins](img19.png)
![jenkins](img20.png)
![jenkins](img21.png)
![jenkins](img22.png)
![jenkins](img23.png)
![jenkins](img24.png)
![jenkins](img25.png)
![jenkins](img26.png)

```
cd ~/openshift-aug-2026
git pull
cd Day5/CICD
oc project jegan-project
oc apply -f buildconfig.yml
oc get buildconfigs

oc create imagestream hello-microservice
oc policy add-role-to-user edit system:serviceaccount:jegan-project:default
oc start-build bc/java-app-pipeline

oc logs -f bc/java-app-pipeline 
```

## Certifications Recommended

#### Recommended for Developers
<pre>
- To prepare for EX288 Certification, you may attend the training that covers topics listed in 
  Containers & Kubernetes Fundamentals (DO188) and Red Hat OpenShift Development II: Building Kubernetes Applications (DO288).
- Red Hat doesn't mandate attending training before taking EX288 Certification, those who are technically hands-on will be able 
  to clear the Certification with consistent preparation
</pre>

<pre>
- Red Hat Certified Specialist in Containers (EX188)
- Red Hat Certified Specialist in OpenShift Application Development (EX288)
- Red Hat Certified Cloud-Native Developer (EX378) - Quarkus/Java Focused
- Red Hat Certified Specialist in OpenShift AI (EX267)
- Event-Driven Development (EX453 ) - Kafka/AMQ Streams
- Building Resilient Microservices EX328 - Service Mesh/Istio
</pre>

#### Recommended for DevOps Engineers
<pre>
- Red Hat Certified Specialist in OpenShift Automation and Integration (EX380)
- Red Hat Certified Cloud-Native Developer (RHCCD)
- Red Hat Certified OpenShift Architect 
- The Foundation: Red Hat Certified Engineer (EX294 - RHCE)
- The Platform: EX288 (Developer) OR EX280 (Admin)
- The Pipeline: EX288 (OpenShift Pipelines)
- The Automation Pinnacle: Red Hat Certified Specialist in MultiCluster Management (EX480)
</pre>

#### Recommended for Administrators
<pre>
- Prerequisite: Red Hat Certified System Administrator (EX200 - RHCSA)
- Core: Red Hat Certified Specialist in OpenShift Administration (EX280)
- Advanced: Red Hat Certified Specialist in OpenShift Automation & Integration (EX380)
- OpenShift Virtualization (EX316) - Running VMs alongside containers
- OpenShift Data Foundation (EX370) - Managing cluster storage/ODF
</pre>
