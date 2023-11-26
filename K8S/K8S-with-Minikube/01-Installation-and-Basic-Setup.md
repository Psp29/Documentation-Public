# Installing Minikube on Arch-Based System.

## Requirements:

- Docker or any other hypervisor. (e.g. VirtualBox, VMWare Workstation, QEMU, Libvert etc.)
- Refer Minikube [Official Docs](https://minikube.sigs.k8s.io/docs/start/) for other OS instructions.
- Install any [AUR helper](https://wiki.archlinux.org/title/AUR_helpers) (yay in this case) **\*Only For Arch Linux**

## Steps:

- Install **MiniKube** and **Kubectl** packages fron Arch User Repository (AUR).

  ```bash
  yay -S minikube
  yay -S kubectl
  ```

- After Installing both the packages, if using docker then make sure the docker context is default.

  ```bash
  docker context list
  ```

  ![](__assets__/Screenshot%20from%202023-03-12%2015-19-43.png)

  If not, then use the default one with command

  ```bash
  docker context use default
  ```

- Now start minikube.

  ```bash
  minikube start
  ```

  ![](__assets__/Screenshot%20from%202023-03-12%2015-24-32.png)

- Check whether the minikube container is running or not.

  ```bash
  docker ps
  ```

  ![](__assets__/Screenshot%20from%202023-03-12%2015-25-30.png)

- To get minikube status.

  ```bash
  minikube status
  ```

  ![](__assets__/Screenshot%20from%202023-03-12%2015-34-53.png)

- Verify Kubernetes Cluster Status.

  ```bash
  kubectl cluster-info
  ```

  ![](__assets__/Screenshot%20from%202023-03-12%2015-36-57.png)

- To get Nodes.
  ```bash
  kubectl get nodes
  ```
  ![](__assets__/Screenshot%20from%202023-03-12%2015-37-11.png)
