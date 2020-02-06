# k8s-fleetman
practice kubernetes project

## Environment variables to export
  ```bash
  AWS_ACCESS_KEY_ID=$(aws configure get aws_access_key_id)
  export AWS_SECRET_ACCESS_KEY=$(aws configure get aws_secret_access_key)
  export NAME=fleetman.k8s.local
  export KOPS_STATE_STORE=s3://zaki-k8s-state-storage
  ```

## Starting the cluster
  _If creating cluster from a new machine, first create ssh keys to be used by kops._
  ```bash
  ssh-keygen -b 2048 -t rsa -f ~/.ssh/id_rsa
  kops create secret --name ${NAME} sshpublickey admin -i ~/.ssh/id_rsa.pub
  ```
  _If restarting cluster from a machine thatalready has ssh keys created, jusr=t follow below commands to bring up the cluster._
  ```bash
  kops create cluster --zones ap-south-1a,ap-south-1b,ap-south-1c ${NAME}
  kops edit ig --name=fleetman.k8s.local nodes
    `change following:
    maxSize: 5
    minSize: 3`
  kops update cluster --yes
    This command will create cluster resources in aws.
  ```