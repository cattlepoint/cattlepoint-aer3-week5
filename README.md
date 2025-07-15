# cattlepoint-aer3-week5
Capstone Project for AWS ER - Week 5
#### Finalized July 15th 2025

## Overview
* These instructions are intended for participants of the AWS Engagement Ready Class started in April 2025
* The goal of this week5 final project is to show competency of all the skills learned in the previous weeks of the AWS Engagement Ready Class
* This project assumes that you have access to the eruser315 credentials
* This project also assumes that you are running the latest MacOS and have terminal access sufficient to install local applications

## Prerequisite
### This section is to ensure you have access to the AWS account and the necessary credentials
* Review the public repo cattlepoint/cattlepoint-aer3-week5
* Login to [AWS Account eruser315account](https://eruser315account.signin.aws.amazon.com/console) using username eruser315 and password ***
* [Download AWS Access Keys](https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/security_credentials/access-key-wizard) file eruser315_accessKeys.csv by selecting Command Line Interface (CLI) and I understand the above recommendation and want to proceed to create an access key -> Next

### This section installs the AWS CLI and configures it with the credentials from the CSV file
* If you haven't already done so, setup Homebrew on your MacOS following [these instructions](https://brew.sh/).
```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
* Perform these steps in the MacOS terminal to install the AWS CLI
```sh
brew update && brew install awscli
```
* Next we need to configure the AWS CLI with the credentials from the CSV file. You can do this by running the following command in your terminal:
```sh
aws configure --profile eruser315
```
* When prompted, enter the access key and secret access key from the CSV file.  Set the default region to us-east-1 and the output format to json.  The command will look like this:
```sh
% aws configure --profile eruser315
AWS Access Key ID [****************GFJ2]:
AWS Secret Access Key [****************Ya3D]:
Default region name [us-east-1]:
Default output format [json]:
```
* Verify AWS credentials are working:
```sh
export AWS_PROFILE=eruser315
aws sts get-caller-identity
```
* Visually verify in output: arn:aws:iam::***:user/eruser315

### This section installs the latest version of the git, github client, AWS Command Line Tool, Kubernetes Command Line Tool, Amazon Elastic Kubernetes Service (Amazon EKS) Command Line Tool (eksctl), Helm, opentofu, Podman (Docker) and verifies they are working

* Perform these steps in the MacOS terminal to install podman, git, github client, kubectl, and eksctl:
```sh
brew update && brew install \
  git            # version control
  gh             # GitHub CLI (repos & Actions)
  awscli         # AWS API access
  kubectl        # Kubernetes control
  eksctl         # spin up/teardown EKS clusters
  helm           # Kubernetes package/upgrade manager
  opentofu       # Infrastructure-as-Code for cluster & app
  podman         # container build/run (or replace with docker)
  jq yq          # JSON/YAML helpers (pipeline scripts)
```

* Login to your github account following the instructions the below command provides:
```sh
gh login
```
* Verify the the github client is working:
```sh
gh auth status
```
* Expected output (contents will vary):
```sh
✓ Logged in to github.com account
```
* Verify the git tool is working:
```sh
git --version
```
* Verify the the Kubernetes CLI is working:
```sh
kubectl version --client
```
* Verify the the eksctl is working:
```sh
eksctl version
```
* Verify podman is working:
```sh
podman -v
```

### This section is to ensure you have git configured and working properly
* Verify you have configured git with your name and email address.
```sh
git config --global --list
```

* Expected output (contents will vary):
```sh
% git config --global --list
user.email=nobody@nowhere.local
user.name=cattlepoint
```

* If you have not done so, run the following commands in your terminal (use your username and email address):
```sh
git config --global user.email "nobody@nowhere.local"
git config --global user.name "cattlepoint"
```
* Clone the cattlepoint-aer3-week5 repository from GitHub to your local machine:
```sh
gh repo fork cattlepoint/cattlepoint-aer3-week5 --clone --remote
```
* Expected output (contents will vary):
```sh
% gh repo fork cattlepoint/cattlepoint-aer3-week5 --clone --remote
✓ Forked!
Cloning into 'cattlepoint-aer3-week5'...
```
* Change into the project directory:
```sh
cd cattlepoint-aer3-week5
```

* Verify the git repository is working:
```sh
git status && git config --get-regexp '^(remote|branch)\.'
```

* Expected output (contents will vary):
```sh
% git status && git config --get-regexp '^(remote|branch)\.'
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
remote.origin.url https://github.com/<your-username-here>/cattlepoint-aer3-week5.git
remote.origin.fetch +refs/heads/*:refs/remotes/origin/*
remote.upstream.url https://github.com/cattlepoint/cattlepoint-aer3-week5.git
remote.upstream.fetch +refs/heads/*:refs/remotes/origin/*
branch.master.remote origin
branch.master.merge refs/heads/master
```

## 20 points – Build a CI/CD pipeline
### Overview
* This repo includes a cattlepoint-aer3-week5-pipeline.yaml file that defines the CI/CD pipeline for CodePipeline
* The pipeline is triggered by changes to the git repository

* The pipeline includes the following stages:
  * Source: Pulls the latest code from the git repository
  * Deploy: Deploys the cattlepoint-aer3-week5.yaml stack via CloudFormation

### Steps
#### Create the GitHub App connection in AWS CodeSuite:
* Navigate to the AWS CodeSuite settings console:
  * https://console.aws.amazon.com/codesuite/settings/connections
* Click on "Create connection"
* Select "GitHub" as the connection type
* Give the connection a name, e.g., "cattlepoint-aer3-week5-connection"
* Follow the prompts to authorize AWS CodeSuite to access your GitHub account
* Once the connection is created, you will see a confirmation message with the connection ARN
* Copy the connection ARN for use in the pipeline configuration

#### Create the CodePipeline using the cattlepoint-aer3-week5-pipeline.yaml file:
* Create the CodePipeline using the cattlepoint-aer3-week5-pipeline.yaml file (replace the ARN and username with your own):
```sh
aws cloudformation create-stack \
  --stack-name rhsi-crm \
  --template-body file://cattlepoint-aer3-week5-pipeline.yaml \
  --capabilities CAPABILITY_IAM \
  --parameters \
    ParameterKey=ConnectionArn,ParameterValue=<your-github-connection-arn> \
    ParameterKey=FullRepositoryId,ParameterValue=<your-username>/cattlepoint-aer3-week5
```

* Verify the pipeline is created successfully:
```sh
% aws cloudformation describe-stacks \
  --stack-name rhsi-crm \
  --query "Stacks[0].StackStatus" \
  --output text
CREATE_COMPLETE
```

* Wait a few minutes and check that the pipeline is running:
```sh
% aws codepipeline get-pipeline-state \
  --name rhsi-crm-pipeline \
  --query 'stageStates[0].latestExecution.status' \
  --output text
Succeeded
```

* Check the results of the latest pipeline execution:
```sh
% aws codepipeline list-pipeline-executions \
  --pipeline-name rhsi-crm-pipeline \
  --max-items 1 \
  --query 'pipelineExecutionSummaries[0].status' \
  --output text
Succeeded
None
```

* Check the status of the cloudformation stack that was created by the pipeline (this will take 20 minutes or so to complete):
```sh
% aws cloudformation describe-stacks \
  --stack-name rhsi-crm-stack \
  --query "Stacks[0].StackStatus" \
  --output text
CREATE_COMPLETE
```

## 30 points – Containerize and test your application to work in an AWS environment
### Overview
* The application Infrastructure as Code (IaC) is stored in a git repository.
* The containers are then built and stored in Elastic Container Registry (ECR).
* The use of ECR allows for easy upgrade/downgrade by tagging the container images with a version number.
* The application is then deployed to an Amazon Elastic Kubernetes Service (EKS) cluster.

### Steps
* connect to the EC2 instance that is running (manually update the security group as needed):
```sh
aws ec2-instance-connect ssh --connection-type direct --instance-id $(aws ec2 describe-instances --filters "Name=tag:Project,Values=rhsi-crm-stack" "Name=instance-state-name,Values=running" --query "Reservations[].Instances[].InstanceId" --output text) --instance-ip $(aws ec2 describe-instances --filters "Name=tag:Project,Values=rhsi-crm-stack" "Name=instance-state-name,Values=running" --query "Reservations[].Instances[].NetworkInterfaces[].Ipv6Addresses[].Ipv6Address" --output text)
```

* Once connected, verify the nodes, pods and svc:
```sh
kubectl get nodes -o wide            # NODES, STATUS, ROLES, AGE, VERSION
kubectl get pods -o wide             # PODS, STATUS, RESTARTS, AGE, IP, NODE
kubectl get pvc mariadb-pvc          # STATUS Bound
kubectl get pods -l app=mariadb      # READY 1/1
kubectl get pods -l app=suitecrm     # both replicas READY
kubectl get svc suitecrm             # NLB hostname present
kubectl get svc suitecrm -o jsonpath='{.status.loadBalancer.ingress[0].hostname}' # NLB hostname
```

* Get the load balancer address:
```sh
% kubectl get svc rhsi-crm-svc -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'
```

* Open the load balancer address in a web browser to access the application.

## 20 points – Automate the construction of the infrastructure/application using Infrastructure as Code (IaC)
### Overview
* EKS is built using using the cattlepoint-aer3-week5.yaml CloudFormation file
* The application itself is deployed via a userdata script
* EKS can scale up and down based on the number of pods running
* The application can be deployed to a different region or cluster for Disaster Recovery (DR) purposes

## 10 points – Test pipeline with a Blue/Green deployment and a rolling update
### Overview
* The pipeline includes a Blue/Green deployment strategy by:
  * updating the CloudFormation template
  * pushing the change to github
  * then powering off the StackName instance
  * asg will detect the instance failure
  * and replace it with a new instance
  * that reapplies the updated Kubenetes Deployment
  * manual intervention via kubectl rollout undo deployments/suitecrm

### Example update:
* Update the frontend Dockerfile and publish it
* Login to the instance
* cd into the project directory:
```sh
cd ~/cattlepoint-aer3-week5/frontend/
git pull
docker build -t rhsi-crm-stack-frontend:latest .
docker tag rhsi-crm-stack-frontend:latest "<account>.dkr.ecr.us-east-1.amazonaws.com/<stackname>-frontend:latest"
docker push "<account>.dkr.ecr.us-east-1.amazonaws.com/<stackname>-frontend:latest"
kubectl get pods -l app=suitecrm -o wide
kubectl rollout restart deployment suitecrm
kubectl rollout status deployment suitecrm
kubectl get pods -l app=suitecrm -o wide
```
* example output (will vary):
```sh
$ cd cattlepoint-aer3-week5/frontend/
[ec2-user@ip-10-0-1-203 frontend]$ kubectl get pods -l app=suitecrm -o wide
kubectl rollout restart deployment suitecrm
kubectl rollout status deployment suitecrm
kubectl get pods -l app=suitecrm -o wide
NAME                        READY   STATUS    RESTARTS   AGE     IP          NODE                  NOMINATED NODE   READINESS GATES
suitecrm-64cd65459c-n2sdl   1/1     Running   0          4m53s   10.0.1.68   i-063367c8e0a274043   <none>           <none>
suitecrm-64cd65459c-tz2c8   1/1     Running   0          4m55s   10.0.1.67   i-063367c8e0a274043   <none>           <none>
deployment.apps/suitecrm restarted
Waiting for deployment "suitecrm" rollout to finish: 1 out of 2 new replicas have been updated...
Waiting for deployment "suitecrm" rollout to finish: 1 out of 2 new replicas have been updated...
Waiting for deployment "suitecrm" rollout to finish: 1 out of 2 new replicas have been updated...
Waiting for deployment "suitecrm" rollout to finish: 1 old replicas are pending termination...
Waiting for deployment "suitecrm" rollout to finish: 1 old replicas are pending termination...
deployment "suitecrm" successfully rolled out
NAME                        READY   STATUS      RESTARTS   AGE     IP          NODE                  NOMINATED NODE   READINESS GATES
suitecrm-64cd65459c-n2sdl   0/1     Completed   0          4m56s   10.0.1.68   i-063367c8e0a274043   <none>           <none>
suitecrm-8586b4fb84-4r28n   1/1     Running     0          2s      10.0.1.64   i-063367c8e0a274043   <none>           <none>
suitecrm-8586b4fb84-plqzt   1/1     Running     0          1s      10.0.1.65   i-063367c8e0a274043   <none>           <none>
```

## 20 points – Design an application and infrastructure in AWS
### Overview
* Customer Relationship Management (CRM) Application: An internal CRM system that is used by the sales team of about 500 people worldwide to help them track all communications and relationships with their leads and clients.
* This design confirms to the Twelve-Factor App methodology.
* This design includes networking, security, and storage system functions.
* This is a microservice architecture.
* Elasticity, high availability, and fault tolerance is included.
* The design uses EKS pods for containerization.
* The design uses EKS Auto Mode for scaling.

## Bonus section
### Cleanup
* Delete the SuiteCRM deployment:
```sh
kubectl delete deployment mariadb suitecrm
kubectl delete pvc mariadb-pvc
kubectl delete -f ~/suitecrm.yaml
```
* Delete the ECR images via the console in both repos
* Delete the StackName-stack via the console
* Delete the StackName CodePipeline bucket contents via the console
* Delete the StackName CodePipeline CloudFormation stack via the console
