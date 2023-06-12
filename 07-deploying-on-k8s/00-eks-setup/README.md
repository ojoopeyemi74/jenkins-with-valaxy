# setting up Eks 
first we create a bootstrap Ec2 instance we will be setting up our eks enviroment on the Ec2

- next : we SSH to the Ec2 instance 

- next: we check the aws --version , to check the version of the CLI
aws eks recommend version 2.2

https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

- after installing the aws version, you can exit and log back in on the EC2 for the version to be updated

# we will be installing Kubectl 

https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html

you should copy the latest version and instal using the cli 


- next: we add execution permission to kubectl
$ chmod +x ./kubectl

- next: - next we move kubectl to /usr/local/bin  
$ mv kubectl /usr/local/bin

- next: kubectl version to check kubectl is installed

# we are installing EKSCTL 

https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html

sudo mv /tmp/eksctl /usr/local/bin

eksctl version


# we need to create an IAM role
- next: we go to IAM on aws dashboad
- next: we create a role (eks-role)- AWS server- EC2 - 
- next: we will be adding permission
AmazonEC2FullAccess
AWSCloudFormationFullAccess
IAMFullAccess
AdministratorAccess

- next: we add the new role to our EC2 bootstrap instance (eks-role)

# we will create EKS cluster and nodes from the EC2 bootstrap image

eksctl create cluster --name cluster-name  \
--region region-name \
--node-type instance-type \
--nodes-min 2 \
--nodes-max 2 \ 
--zones <AZ-1>,<AZ-2>

example:
eksctl create cluster --name opeyemi-cluster \
--region eu-west-2 \
--node-type t2.small \


NOTE: by default the cluster will be created with 2 nodes

# to delete cluster 
eksctl delete cluster opeyemi-clusteor --region eu-west-2