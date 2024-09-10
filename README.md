# Desplegar una aplicación con su base de datos en Kubernetes (AWS EKS) 

## Getting Started
fork and clone https://github.com/edoradoda/cd12355-microservices-aws-kubernetes-project-starter

## ** Coworking app**

Deploy an application called coworking with its database in Kubernetes on Amazon Web Service AWS.

The Coworking aplication Service is a set of APIs that enables users to request one-time tokens and administrators to authorize access to a coworking space. This service follows a microservice pattern and the APIs are split into distinct services that can be deployed and managed independently of one another.

## **Introduction**

This project contains the step-by-step instructions for deploying an application and its database on Kubernetes for the Amazon AWS cloud.

## **Dependencies**
Tools that must be installed in the console:

aws cli: to access the AWS cloud
eksctl: used to create EKS clusters
kubectl : to interact with the cluster in EKS
command to create a cluster if you don't have one:
**`eksctl create cluster --name <MY-CLUSTER> --region us-east-1 --nodegroup-name starter-nodes --node-type t3.small --nodes 1 --nodes -min 1 --nodes-max 2`**

## **Installation**

To install Coworking app , follow these steps:

1. Clone the repository:fork and clone **`git clone https://github.com/edoradoda/cd12355-microservices-aws-kubernetes-project-starter.git`**
2. Navigate to the project directory: **`cd cd12355-microservices-aws-kubernetes-project-starter`**
3. Configure a Database for the Service:
**`kubectl apply -f deployment/pvc.yaml`**
**`kubectl apply -f deployment/pv.yaml`**
**`kubectl apply -f deployment/postgresql-deployment.yaml`**
**`kubectl apply -f deployment/postgresql-service.yaml`** 
Connecting via Port Forwarding:
# List the services
**`kubectl get svc`**
# Set up port-forwarding to `postgresql-service`
**`kubectl port-forward service/postgresql-service 5433:5432 &`**
3. Install dependencies: **`npm install`**
4. Build the project: **`npm run build`**
5. Start the project: **`npm start`**

## **Usage**

To use Project Title, follow these steps:

1. Open the project in your favorite code editor.
2. Modify the source code to fit your needs.
3. Build the project: **`npm run build`**
4. Start the project: **`npm start`**
5. Use the project as desired.

## **Contributing**

If you'd like to contribute to Project Title, here are some guidelines:

1. Fork the repository.
2. Create a new branch for your changes.
3. Make your changes.
4. Write tests to cover your changes.
5. Run the tests to ensure they pass.
6. Commit your changes.
7. Push your changes to your forked repository.
8. Submit a pull request.

## **Contact**

If you have any questions or comments about Coworking app, please contact **[Eduar](doradoeduar@gmail.com)**.

## **Conclusion**

That's it! This is a basic template for a proper README file for a general project. You can customize it to fit your needs, but make sure to include all the necessary information. A good README file can help users understand and use your project, and it can also help attract contributors.