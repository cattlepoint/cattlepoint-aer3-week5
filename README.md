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

### This section installs the latest version of the Kubernetes Command Line Tool, Amazon Elastic Kubernetes Service (Amazon EKS) Command Line Tool (eksctl), Podman (Docker, project dependencies and verifies they are working
* Perform these steps in the MacOS terminal to install podman, git, github client, kubectl, and eksctl:
```sh
brew update && brew install git gh kubectl eksctl podman
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

* If you have not done so, run the following commands in your terminal:
```sh
git config --global user.email "nobody@nowhere.local"
git config --global user.name "cattlepoint"
```
* Clone the cattlepoint-aer3-week5 repository from GitHub to your local machine:
```sh
gh repo clone cattlepoint/cattlepoint-aer3-week5
```
* Expected output (contents will vary):
```sh
% gh repo clone cattlepoint/cattlepoint-aer3-week5
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
remote.origin.url https://github.com/cattlepoint/cattlepoint-aer3-week5.git
remote.origin.fetch +refs/heads/*:refs/remotes/origin/*
branch.master.remote origin
branch.master.merge refs/heads/master
```
