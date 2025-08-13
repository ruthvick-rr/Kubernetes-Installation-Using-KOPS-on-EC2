# Kubernetes-Installation-Using-KOPS-on-EC2
Kubernetes Installation on AWS EC2 using kops — Step-by-Step Guide


1. Install Prerequisites Locally

   
Install AWS CLI:


sudo apt-get update && sudo apt-get install -y awscli


aws --version



Install kubectl:


curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"


chmod +x kubectl


sudo mv kubectl /usr/local/bin/


kubectl version --client



Install kops:


curl -Lo kops https://github.com/kubernetes/kops/releases/latest/download/kops-linux-amd64


chmod +x kops


sudo mv kops /usr/local/bin/


kops version

Provide the below permissions to your IAM user. If you are using the admin user, the below permissions are available by default


AmazonEC2FullAccess


AmazonS3FullAccess


IAMFullAccess


AmazonVPCFullAccess

Set up AWS CLI configuration on your EC2 Instance or Laptop.


Run aws configure


Create an S3 Bucket for kops State Storage


aws s3api create-bucket --bucket kops-ruthvick-storage --region us-east-1 (Use Unique Name for Bucket Creation)


Create the cluster


export NAME=cluster.k8s.example.com


export KOPS_STATE_STORE=s3://kops-ruthvick-storage

kops create cluster \
  --name=demok8sruthvickcluster.k8s.local \
  --state=s3://kops-ruthvick-storage \
  --zones=us-east-2a,us-east-2b,us-east-2c \
  --node-count=3 \
  --node-size=t2.micro \
  --control-plane-size=t2.micro \
  --master-volume-size=8 \
  --node-volume-size=8 \
  --dns private
  

Important: Edit the configuration as there are multiple resources created which won't fall into the free tier.


kops edit cluster demoK8sruthvickcluster.k8s.local


Step 12: Build the cluster

kops update cluster demok8sruthvickcluster.k8s.local --yes --state=s3://kops-ruthvick-storage


This will take a few minutes to create............

After a few mins, run the below command to verify the cluster installation.

kops validate cluster demok8sruthvickcluster.k8s.local
