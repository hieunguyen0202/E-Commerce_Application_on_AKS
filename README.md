## Deploy an E-Commerce Project on Azure Kubernetes Service

- Stan's Robot Shop is a sample microservice application you can use as a sandbox to test and learn containerised application orchestration and monitoring techniques. It is not intended to be a comprehensive reference example of how to write a microservices application, although you will better understand some of those concepts by playing with Stan's Robot Shop. To be clear, the error handling is patchy and there is not any security built into the application.

- Video demo: 
- This sample microservice application has been built using these technologies:
  - NodeJS ([Express](http://expressjs.com/))
  - Java ([Spring Boot](https://spring.io/))
  - Python ([Flask](http://flask.pocoo.org))
  - Golang
  - PHP (Apache)
  - MongoDB
  - Redis
  - MySQL ([Maxmind](http://www.maxmind.com) data)

## Architecture

![Deploy an E-Commerce Project on AKS drawio](https://github.com/hieunguyen0202/E-Commerce_Project_on_AKS/assets/98166568/150751ba-2102-43dd-aa10-5b9ca9a1fb40)

## Implementation
#### Create kubernetes cluster on Azure/GCP 
- Choose name `gitops-cluster`
- Enable autoscaling node for min `1` and max `2`, enable for IP of pod

#### Connect to Kubernetes cluster
- For Azure Kubernetes services

  ```
  az aks get-credentials --resource-group resourcegroupname --name clustername
  ```
- For Google Kubernetes Engine

  ```
  gcloud container clusters get-credentials CLUSTER_NAME --region=CLUSTER_REGION
  ```
#### Setup for Helm Chart
- Clone this source code

  ```
  git clone https://github.com/hieunguyen0202/E-Commerce_Project_on_AKS.git
  ```
- Go to `AKS` folder and `helm` folder and inside this folder run this command

  ```
  kubectl create ns robot-shop
  ```
- And package for all manifests file

  ```
  helm install robot-shop --namespace robot-shop .
  ```
- To unintall

  ```
  helm uninstall robot-shop --namespace robot-shop

  ```
- Check some pod and services running

  ```
  kubectl get pods -n robot-shop
  kubectl get svc -n robot-shop
  ```
### If you use GKE cluster under GKE folder
- Under `GKE/helm` folder, create new file

  ```
  vim storage-class.yaml
  ```
- Add information below

  ```
  kind: StorageClass
  apiVersion: storage.k8s.io/v1
  metadata:
    name: default
  provisioner: kubernetes.io/gce-pd
  parameters:
    type: pd-standard
  ```
- Apply to command:

  ```
  kubectl apply -f storage-class.yaml
  ```
- Check status

  ```
  kubectl get pvc -n robot-shop
  ```
- And run this command
  ```
  kubectl create ns robot-shop
  ```
- And package for all manifests file

  ```
  helm install robot-shop --namespace robot-shop .
  ```
#### Create Ingress Service
- Modify this `ingress.yaml` file

  ```
  vim ingress.yaml
  ```

- Create

  ```
  kubectl apply -f ingress.yaml -n robot-shop
  ```

- Check status

  ```
  kubectl get pods -n kube-system
  kubectl get ing -n robot-shop
  ```

