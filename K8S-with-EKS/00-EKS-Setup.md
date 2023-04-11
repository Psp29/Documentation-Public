# Installing Required tools and setting up neccessary permissions and roles.

## Requirements:

- Active AWS account and [IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_console) with sufficient permissions.
- AWS user [Access Keys](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-appendix-sign-up.html).

<br />
<!-- ## Steps: -->

## Install **kubectl**.

- kubectl can be installed on any OS (Refer official [docs](https://kubernetes.io/docs/tasks/tools/)), here we will install on linux (Arch Linux to be specific).
  ```bash
  yay -S kubectl
  ```
- **For Ubuntu or Debian-Based Distributions.**

  Update the apt package index and install packages needed to use the Kubernetes apt repository

  ```bash
  sudo apt-get update
  sudo apt-get install -y ca-certificates curl
  ```

  ```bash
  sudo apt-get install -y apt-transport-https
  ```

  Download the Google Cloud public signing key:

  ```bash
  sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
  ```

  Add the Kubernetes apt repository:

  ```bash
  echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
  ```

  Update apt package index with the new repository and install kubectl:

  ```bash
  sudo apt-get update
  sudo apt-get install -y kubectl
  ```

<br />

## Install AWS-CLI Utility.

- AWS-Cli is available for all the platforms.(Refer offcial [AWS docs](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) on how to install the tool on your specific OS.)
- Here we will follow the instructions for **Linux**.
- Install pre-requisites (Unzip tool ignore if already installed or any equivalent is already present).
  ```bash
  sudo apt install unzip
  ```
- Install AWS-Cli.
  ```bash
  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
  unzip awscliv2.zip
  sudo ./aws/install
  ```
- Verify whether the AWS Cli is installed or not.
  ```bash
  aws --version
  ```
  ![](__assets__/Screenshot%20from%202023-04-10%2023-03-16.png)

<br />

## Configure AWS CLI.

- To use AWS CLI we first have to configure, for that we need IAM User Access Keys.
  ```bash
  aws configure
  ```
  It will then prompt for Access Key, Access Secret Key, Default Region and Output Format (enter _json_ in Output Format).
  Enter the asked details and then move to next step.

<br />

## Create IAM role for EKS Cluster.

- Now we have to create an IAM role for EKS cluster so that it can create the sufficient resources On-Demand automatically. (Such as EBS and EC2, etc.)
- Login to AWS Console and Search for IAM.
- Now Go to **Roles** and click on **Create role**.
  ![](__assets__/Screenshot%20from%202023-04-10%2015-03-14.png)

- Select AWS Service -> Use cases for other AWS services -> EKS -> EKS-Cluster -> next
  ![](__assets__/Screenshot%20from%202023-04-10%2015-04-34.png)

- Review the policy name and click next.
  ![](__assets__/Screenshot%20from%202023-04-10%2015-05-03.png)

- Enter appropriate name and then click **Create Role**
