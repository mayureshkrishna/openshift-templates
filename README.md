# OpenShift Application Templates


## 1. Microservice Templates

The first set of templates are for MicroServices. These are generic templates with sizing and allows team/developers to build their own application and they should base it Alpine and Slim JDK verison:

```
FROM mayureshkrishna/java-8:server-jre-8u121-slim
```

### (i) Microservice Small Template

This template makes following resources available on the containers:
1. ImageStream
2. DeploymentConfig
    a. Rolling Deployment Strategy
    b. Config Change as Trigger
    c. Container Resources limited to CPU: 500m to 1 Core and Memory: 100Mi to 500Mi and Port 8080
3. Service exposed on port 8080
4. Route with TLS edge termination
5. HorizontalPodAutoscaler with Min 1 and Max 4 pods with 80% CPU utilization as trigger

This template has 4 parameters:
1. APPLICATION_NAME - This is the name of application/microservice
2. ENV_NAME - This is the project name on OpenShift
3. APPLICATION_DOMAIN - This the sub-domain of your OpenShift deployment
4. REGISTRY_URL - This is OpenShift internal docker registry URL

#### Following command can be used to create the template in openshift:

```
oc create -f microservice-small-template.json -n dev
```

Your OpenShift admin can create this template at OpenShift level thus making it available across all projects.

#### Following command can be used to update/replace an existing template in openshit:

```
oc replace -f microservice-small-template.json -n dev
```

#### Following command can be used to export an existing template in openshit to a file:

```
oc export templates/microservice-small-template -o json > microservice-small-template.json -n dev
```

You can use yaml instead of json if you prefer yaml.

#### Following command can be used to create a new app using this template in openshit:

```
oc new-app --template=microservice-small-template -p APPLICATION_NAME=sample-app,ENV_NAME=dev,APPLICATION_DOMAIN=amnotion.com,REGISTRY_URL=172.30.36.202:5000 -l "app=sample-app"  -n dev
```
