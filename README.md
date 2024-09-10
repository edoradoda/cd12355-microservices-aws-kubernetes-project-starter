# Coworking app 
Deploy an application called coworking with its database in Kubernetes on Amazon Web Service AWS.

The Coworking aplication Service is a set of APIs that enables users to request one-time tokens and administrators to authorize access to a coworking space. This service follows a microservice pattern and the APIs are split into distinct services that can be deployed and managed independently of one another.

This project contains the step-by-step instructions for deploying an application and its database on Kubernetes for the Amazon AWS cloud.

<p > <a href="https://aws.amazon.com" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/amazonwebservices/amazonwebservices-original-wordmark.svg" alt="aws" width="40" height="40"/> </a> <a href="https://www.docker.com/" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/docker/docker-original-wordmark.svg" alt="docker" width="40" height="40"/> </a> <a href="https://git-scm.com/" target="_blank" rel="noreferrer"> <img src="https://www.vectorlogo.zone/logos/git-scm/git-scm-icon.svg" alt="git" width="40" height="40"/> </a> <a href="https://kubernetes.io" target="_blank" rel="noreferrer"> <img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" alt="kubernetes" width="40" height="40"/> </a> <a href="https://www.linux.org/" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/linux/linux-original.svg" alt="linux" width="40" height="40"/> </a> <a href="https://www.postgresql.org" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/postgresql/postgresql-original-wordmark.svg" alt="postgresql" width="40" height="40"/> </a> <a href="https://www.python.org" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg" alt="python" width="40" height="40"/> </a> </p>

## **Dependencies**
Tools that must be installed in the console:

aws cli: to access the AWS cloud
eksctl: used to create EKS clusters
kubectl : to interact with the cluster in EKS
command to create a cluster if you don't have one:
```bash
eksctl create cluster --name <MY-CLUSTER> --region us-east-1 --nodegroup-name starter-nodes --node-type t3.small --nodes 1 --nodes -min 1 --nodes-max 2
```

## **Installation**

To install Coworking app , follow these steps:

1. Clone the repository:fork and clone:
```bash
git clone https://github.com/edoradoda/cd12355-microservices-aws-kubernetes-project-starter.git
```
2. Navigate to the project directory: 
```bash
cd cd12355-microservices-aws-kubernetes-project-starter
```
3. Configure a Database for the Service:
```bash
kubectl apply -f deployment/pvc.yaml
kubectl apply -f deployment/pv.yaml
kubectl apply -f deployment/postgresql-deployment.yaml
kubectl apply -f deployment/postgresql-service.yaml 
```

Connecting to DB via Port Forwarding:
```bash
# List the services
kubectl get svc
# Set up port-forwarding to `postgresql-service`
kubectl port-forward service/postgresql-service 5433:5432 &
```

Run Seed Files
```bash
# instal psql to connect to DB
apt update
apt install postgresql postgresql-contrib
#run the seed files in db/ directory in order to create the tables and populate them with data.
export DB_PASSWORD=mypassword
PGPASSWORD="$DB_PASSWORD" psql --host 127.0.0.1 -U myuser -d mydatabase -p 5433 < <FILE_NAME.sql>
```
Checking the tables
```bash
PGPASSWORD="$DB_PASSWORD" psql --host 127.0.0.1 -U myuser -d mydatabase -p 5433
select *from users;
\q
```

3. Deploy the Analytics Application: 
```bash
docker build -t test-coworking-analytics .
#Verify the Docker Image
docker run --network="host" test-coworking-analytics
#The endpoint should respond with data form DB
curl  http://127.0.0.1:5153/api/reports/user_visits 
```
4. Continuous Integration with CodeBuild:
The purpose of this step is to provide a systematic approach to pushing the Docker image of the coworking application into Amazon ECR.

First, create an Amazon ECR repository on your AWS console.

Then, create an Amazon CodeBuild project that is connected to your project's GitHub repository.

Once they are done, modify buildspec.yaml file that will be triggered whenever the project repository is updated

5. Deploy the Application:
```bash
# load the env  variables and secret password
kubectl apply -f deployment/configmap.yaml
#Verify the creation of the configmap
kubectl get configmaps

# deploy the app
kubectl apply -f deployment/coworking.yaml
# You cantest the app with URL from load balancer, e.g.:
curl http://ad9373834b3964a1b98e7c36a4944852-1498505752.us-east-1.elb.amazonaws.com:5153/api/reports/daily_usage
```
6. Setup CloudWatch Logging:

```bash
# Add aditional permisions to installa addon  Cloudwatch
aws iam attach-role-policy \
--role-name <ROLE-NODE-GROUP-EKS> \
--policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy \
--region <MY-REGION>
# Create addon
aws eks create-addon --addon-name amazon-cloudwatch-observability --cluster-name <MY-CLUSTER-NAME>
```
Now go to the Amazon console and you will be able to see the detailed logs of the application by accessing the CloudWatch service -> Metrics.

## **Contact**

If you have any questions or comments about Coworking app, please contact **[Eduar](doradoeduar@gmail.com)**.


