# EKS Deployment.

## Requirements:

- Working EKS Cluster with Nodes.

<br />

## Add Cluster to kubectl config.

- If you have AWS-CLI installed and configured (which we have) it is quite easy to add EKS Cluster to kubectl contol.
- Run following commands.

  Following command will create a kubeconfig.

  ```bash
  aws eks update-kubeconfig --region <aws-region-code> --name <cluster-name>
  ```

  ![](__assets__/Screenshot%20from%202023-04-07%2015-11-44.png)

  Run following command to verify that the kubectl configured properly.

  ```bash
  kubectl config get-contexts
  ```

  ![](__assets__/Screenshot%20from%202023-04-07%2015-12-48.png)

  Verify whether the node is connected or not.

  ```bash
  kubectl get nodes
  ```

  ![](__assets__/Screenshot%20from%202023-04-07%2015-13-09.png)

<br />

## Install EBS CSI Driver

It is a vital step if we want to use EBS as persistant volumes and are managed by AWS.

- Run following command to add user access keys as Secret.
  ```bash
  kubectl create secret generic aws-creds -n kube-system \
  --from-literal=aws_access_key_id=<Access Key ID> \
  --from-literal=aws_secret_access_key=<Access Secret Key>
  ```
- To install/Deploy EBS-CSI driver run following command.

  ```bash
  kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=master"
  ```

  Check if the EBS-CSI driver is installed or not.

  ```bash
  kubectl get pods -n kube-system | grep ebs
  ```

  ![](__assets__/Screenshot%20from%202023-04-10%2015-19-23.png)

- When deploying Kubernetes deployments and StatefulSets on EKS we have multiple Persistent storage options such as EBS (Elastic Block Storage) and EFS (Elastic File Storage which is a NFS).
- Here we will be using EBS as Persistent storage.
- Using this method we have advantage of Not creating and managing the PV (Persistent Volumes) by ourselves. This will be handled by EKS node.
- The only difference between Manual storage and EBS is that we donot need to create PV (Persistent Volumes) instead we create **Storage Class** and mention StorageClassName in PVC (Persistent Volume Claim).

<br />

## Create Storage Class and PVC (Persistent Volume Claim).

- Here we deployed Wordpress Application so we need 2 PVC (for Wordpress itself and one for MySQL).
- Storage Class For Wordpress (Storage-Class-WP.yaml).

  ```yaml
  kind: StorageClass
  apiVersion: storage.k8s.io/v1
  metadata:
    name: <WP-storage-Class-Name>
    namespace: <Your namespace>
  provisioner: ebs.csi.aws.com
  parameters:
    type: gp2
    encrypted: "true"
    fsType: ext4
  reclaimPolicy: Delete
  ```

- Storage Class For MySQL DB (Storage-Class-DB.yaml).

  ```yaml
  kind: StorageClass
  apiVersion: storage.k8s.io/v1
  metadata:
    name: <db-storage-class-name>
    namespace: <Your namespace>
  provisioner: ebs.csi.aws.com
  parameters:
    type: gp2
    encrypted: "true"
    fsType: ext4
  reclaimPolicy: Delete
  ```

- PVC for Wordpress Deployment.

  ```yaml
  apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
    name: wp-claim
    namespace: <Your namespace>
    labels:
      app: wp
  spec:
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 1Gi
    storageClassName: <WP-storage-Class-Name>
  ```

- PVC for MySQL StatefulSet.

  ```yaml
  apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
    name: wp-db-claim
    namespace: <Your namespace>
    labels:
      app: wp-db
  spec:
    accessModes:
      - ReadWriteOnce
    storageClassName: <db-storage-class-name>
    resources:
      requests:
        storage: 1Gi
  ```

- After creating the Storage class and PVC deploy them in EKS cluster with command.

  ```bash
  kubectl create -f <filename.yaml>
  ```

  \*note: run command for each file. or run

  ```bash
  kubectl create -f ./
  ```

- After Deploying all other reources we can verify by running following command.

  ```bash
  kubectl get all -n <Your namespace>
  ```

  ![](__assets__/Screenshot%20from%202023-04-10%2015-20-28.png)

- Now access your deployment on browser with the Load Balancer url.
