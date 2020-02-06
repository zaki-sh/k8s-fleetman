# k8s-fleetman
Practice kubernetes project

## Prerequisites
1. [aws-cli](https://github.com/aws/aws-cli "aws-cli github repo")
2. [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
3. [kops](https://github.com/kubernetes/kops#installing)

## Getting Started with kops on AWS
1. Setup `aws configure` with user access keys that has permissions to create new user and group.
2. [Setup `kops` IAM user](https://github.com/kubernetes/kops/blob/master/docs/getting_started/aws.md#setup-iam-user)
3. Run `aws configure` again and setup access keys created for `kops` aws iam user.
4. [Cluster State storage](https://github.com/kubernetes/kops/blob/master/docs/getting_started/aws.md#cluster-state-storage) 
5. [Creating your first cluster](https://github.com/kubernetes/kops/blob/master/docs/getting_started/aws.md#creating-your-first-cluster) <!-- svg jekyll helper -->
{% octicon arrow-down %}
##### Prepare local environment
  ```bash
  export AWS_ACCESS_KEY_ID=$(aws configure get aws_access_key_id)
  export AWS_SECRET_ACCESS_KEY=$(aws configure get aws_secret_access_key)
  export NAME=fleetman.k8s.local
  export KOPS_STATE_STORE=s3://<s3 bucket name will go here>
  ```
##### Create cluster configuration
  If creating cluster from a new machine, first create ssh keys to be used by kops.
  ```bash
  ssh-keygen -b 2048 -t rsa -f ~/.ssh/id_rsa
  kops create secret --name ${NAME} sshpublickey admin -i ~/.ssh/id_rsa.pub
  ```
  If restarting cluster from a machine that already has ssh keys created, just follow below commands to bring up the cluster.
  *NOTE: to get list of availability zones use `aws ec2 describe-availability-zones --region ap-south-1` and replace ap-south-1 with desired aws region.*
  ```bash
  kops create cluster --zones ap-south-1a,ap-south-1b,ap-south-1c ${NAME}
  kops edit ig --name=fleetman.k8s.local nodes
    change following:
    maxSize: 5
    minSize: 3
  kops update cluster --yes
    This command will create cluster resources in aws.
  ```