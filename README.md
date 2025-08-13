Kubernetes Installation on AWS EC2 using kops — Step-by-Step Guide


Step 1: Install Prerequisites Locally



Install AWS CLI:


sudo apt-get update && sudo apt-get install -y awscli

(If it says E: Package 'awscli' has no installation candidate)


Try Using:

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"


unzip awscliv2.zip


sudo ./aws/install


aws --version


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


Step 2: Provide IAM Permissions


Make sure your IAM user has the following permissions. If you are using the admin user, these are available by default:

AmazonEC2FullAccess

AmazonS3FullAccess

IAMFullAccess

AmazonVPCFullAccess

Step 3: Configure AWS CLI

aws configure


Enter your Access Key, Secret Key, Region (e.g., us-east-2 for Ohio), and output format.

Step 4: Create an S3 Bucket for kops State Storage

aws s3api create-bucket --bucket kops-ruthvick-storage --region us-east-2 --create-bucket-configuration LocationConstraint=us-east-2
(Use a unique bucket name — bucket names are globally unique.)

Step 5: Set Environment Variables

export NAME=demok8sruthvickcluster.k8s.local


export KOPS_STATE_STORE=s3://kops-ruthvick-storage


Step 6: Create the Cluster


kops create cluster \
  --name=$NAME \
  --state=$KOPS_STATE_STORE \
  --zones=us-east-2a,us-east-2b,us-east-2c \
  --node-count=3 \
  --node-size=t2.micro \
  --control-plane-size=t2.micro \
  --master-volume-size=8 \
  --node-volume-size=8 \
  --dns private

  
Step 7: Edit the Configuration (Optional)
Reduce resources or change configurations to control cost.



kops edit cluster $NAME --state=$KOPS_STATE_STORE


Step 8: Build the Cluster

kops update cluster $NAME --yes --state=$KOPS_STATE_STORE


Step 9: Export kubeconfig (if needed)


kops export kubecfg $NAME --state=$KOPS_STATE_STORE


Step 10: Validate the Cluster


kops validate cluster $NAME --state=$KOPS_STATE_STORE


This will set up a Kubernetes cluster in AWS EC2 with 3 worker nodes and 1 control plane, spread across 3 availability zones in the Ohio (us-east-2) region.
