# Cloud Agnostic Deployment - App modernization
This getting started guide is intended to demonstrate how the Cyara apps which comprise of Windows as well as Linux based services can be transformed to allow a more cloud agnostic deployment.

## Architecture Overview
The below architecture diagram is for a more cloud agnostic approach. Using this approach, the deployments can be made even in private datacenter by ensuring we have access to the cloud endpoints for github, dockerhub and Elasticsearch cloud. 
![Alt text](Cyara-App-Modernization-CloudAgnostic.jpeg?raw=true "Cyara Cloud Agnostic Deployment")

Using the cloud agnostic architecture, deployment on AWS cloud using its own managed services would be something as follows- 
![Alt text](Cyara-App-Modernization-AWS.jpeg?raw=true "Cyara AWS Cloud Deployment")

## Step  1: Application Containerization
* The current application is tightly coupled with infra provisioning. It also depends on VM golden images built by Packer. 
* To make it more cloud agnostic, we will build the application as Docker images.
* Each service can be built as a separate docker image using a Dockerfile. Hence we will have approximately 20 docker images corresponding to each service. 
* These docker images can be consistently deployed across any cloud platform. 
* Docker containers are also easier to rollback compared to traditional standalone deployments. 

## Step 2: Use Jenkins for CI/CD
* Jenkins is a popular server for implementing continuous integration and continuous delivery pipelines. 
* For this use case, we will use Jenkins to build a Docker image from a Dockerfile, push that image to the Amazon ECR registry(or Docker Hub). 
* The DEV team can be involved here to provide a task definition json file to define the configuration for container. 
* Using a Jenkins job and Terraform scripts build an ECS cluster(or VM cluster). 
* Finally, run another Jenkins job to deploy and update a service on your ECS cluster(or VM cluster).

## Step 3: Enabling application monitoring
* We can enabled the app monitoring by two ways - 
  * Integrating app monitoring agents in the docker images and pushing the logs to Elasticsearch
  * Use a cloud managed service such as AWS CloudWatch and aggregate all the logs at one place. 
* Depending on the log aggregation location, we can create alerts and dashboard to monitor the application and platform health as well as perform administrative actions. 