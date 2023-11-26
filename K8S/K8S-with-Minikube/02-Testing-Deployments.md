# Perform Simple Nginx Deployment.

## Requirements:

- [Working K8S cluster with minikube.](./01-Installation-and-Basic-Setup.md01)

## Steps:

- Create a new directory and create a new `yaml` file in that directory.

  ```bash
  mkdir -p k8s-projects/nginx-deployment
  cd k8s-projects/nginx-deployment
  nano nginx-deployment.yaml
  ```

  and add following configurations.

  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: nginx-deployment
    labels:
      app: nginx
  spec:
    selector:
      matchLabels:
        app: nginx
    template:
      metadata:
        labels:
          app: nginx
      spec:
        containers:
        - name: nginx
          image: nginx:1.14
          ports:
          - containerPort: 80

  apiVersion: v1
  kind: Service
  metadata:
    name: nginx-service
    labels:
      run: nginx-service
  spec:
    type: NodePort
    ports:
    - port: 80
      protocol: TCP
    selector:
      app: nginx
  ```

  save and exit.

- Create the deployment by running following `kubectl` command.

  ```bash
  kubectl create -f nginx-deployment.yaml
  ```

  ![](__assets__/Screenshot%20from%202023-03-12%2015-56-08.png)

- Verify that the deployment is up and running.

  ```bash
  kubectl get deployments
  ```

  ![](__assets__/Screenshot%20from%202023-03-12%2015-56-28.png)

  and to check if nginx running or not on the browser.

  ```bash
  kubectl get svc
  minikube service nginx-service --url
  ```

  ![](__assets__/Screenshot%20from%202023-03-12%2015-57-20.png)
  ![](__assets__/Screenshot%20from%202023-03-12%2016-02-05.png)

### Minikube Dashboard:

- Start a minikube dashboard.
  ```bash
  minikube dashboard
  ```
  ![](__assets__/Screenshot%20from%202023-03-12%2016-04-07.png)
  ![](__assets__/Screenshot%20from%202023-03-12%2016-04-21.png)
