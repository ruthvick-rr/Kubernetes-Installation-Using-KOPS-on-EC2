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

Create the cluster
kops create cluster --name=demok8scluster.k8s.local --state=s3://kops-abhi-storage --zones=us-east-1a --node-count=1 --node-size=t2.micro --master-size=t2.micro  --master-volume-size=8 --node-volume-size=8
Important: Edit the configuration as there are multiple resources created which won't fall into the free tier.
kops edit cluster myfirstcluster.k8s.local
Step 12: Build the cluster

kops update cluster demok8scluster.k8s.local --yes --state=s3://kops-abhi-storage
This will take a few minutes to create............

After a few mins, run the below command to verify the cluster installation.

kops validate cluster demok8scluster.k8s.local
