# Day 5

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
