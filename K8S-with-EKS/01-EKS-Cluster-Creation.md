# EKS Cluster Creation.

## Requirements:

- AWS cli configured.
- AWS EKS role.

<br />

## Create An EKS Cluster.

- Login to the AWS Console as the IAM user (**\*Important**) because, if we create cluster as root user then we cannot control/access cluster from the terminal (kubectl). Basically make sure that we are using one user for all the cluster related activities in both console and on kubectl/aws-cli.
- Search for EKS -> Create cluster.
- Configure cluster -> Name -> select Cluster Role (the role we created earlier.)
  ![](__assets__/Screenshot%20from%202023-04-07%2014-53-06.png)
- Select VPC -> Select Subnets -> Security Groups -> Next.
  ![](__assets__/Screenshot%20from%202023-04-07%2014-53-22.png)
- Then click next a bunch of times unless you want ot change anything specific.
- Review the configuration and Click on create.

<br />

## Create an EKS **Node** Role.

- Search for IAM in AWS console.
- Click roles -> Create Role.
  ![](__assets__/Screenshot%20from%202023-04-11%2012-02-01.png)
- Select trusted entity as **AWS Service** -> Under Use case select **EC2** -> Next.
  ![](__assets__/Screenshot%20from%202023-04-11%2012-02-32.png)
- On next tab filter and select following 3 Permission Policies then click Next.

  - AmazonEKSWorkerNodePolicy
  - AmazonEC2ContainerRegistryReadOnly
  - AmazonEBSCSIDriverPolicy

  ![](__assets__/Screenshot%20from%202023-04-11%2012-06-31.png)

- Enter a Role Name -> Review Policies below -> click Create role.
  ![](__assets__/Screenshot%20from%202023-04-11%2012-07-55.png)
  ![](__assets__/Screenshot%20from%202023-04-11%2012-07-58.png)

<br />

## Create a Node-Group.

- Search EKS in AWS Console -> Select created cluster.
- In Cluster overview page we can see a **compute** tab click on it.
- Click on Add node group.
  ![](__assets__/Screenshot%20from%202023-04-07%2015-03-27.png)
- Configre node group -> Enter a suitable name -> Select the Node IAM role that we created before.
  ![](__assets__/Screenshot%20from%202023-04-07%2015-03-39.png)
- Select Instance Type -> AMI Type -> Capacity Type -> Disk Size -> Scaling -> Next.
  ![](__assets__/Screenshot%20from%202023-04-07%2015-04-02.png)
- Select Subnets -> Enable SSH Access (if needed) -> Next.
  ![](__assets__/Screenshot%20from%202023-04-07%2015-04-46.png)
- Revies and Create.
  ![](__assets__/Screenshot%20from%202023-04-07%2015-07-03.png)
- Done.
