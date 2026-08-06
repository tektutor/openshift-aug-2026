# Day 4

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
