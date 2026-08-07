# Day 5

## Lab - JMS Producer and JMS Consumer microservices communication via AMQ
```
oc delete project jegan-project

oc new-project jegan-project

oc new-app --name=jms-producer https://github.com/tektutor/openshift-aug-2026.git --context-dir=Day5/jms-demo/producer --strategy=docker
oc new-app --name=jms-consumer https://github.com/tektutor/openshift-aug-2026.git --context-dir=Day5/jms-demo/consumer --strategy=docker

oc get pods

oc logs jms-producer-5f78665c77-8pchq
oc logs jms-consumer-695fdfd78c-gcrlg
```
