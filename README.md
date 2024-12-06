
**CosmoCloud Deploy - Kubernetes App Deployment with Helm**

**Technologies Used:**

- **Kubernetes**: An open-source platform for automating the deployment, scaling, and management of containerized applications.
- **Minikube**: A tool to run Kubernetes clusters locally for development and testing.
- **Helm**: A package manager for Kubernetes that simplifies the deployment of applications on a Kubernetes cluster.
- **Docker**: Containerized application images (for Backend, Frontend, and Redis).

**Setup Instructions**

**1. Set Up Minikube and Kubernetes**

**1.1 Install Minikube**

Ensure that you have **Minikube** installed on your machine. Follow the installation guide for your operating system

**1.2 Start Minikube**

After Minikube is installed, start a local Kubernetes cluster:

minikube start

Once Minikube starts, verify that the cluster is running:

kubectl get nodes

You should see a node with the name minikube in the Ready state.


**2. Create the Helm Chart**

Now that your Kubernetes cluster is up and running, let's create the **Helm Chart** that will deploy our application stack.

**2.1 Create a New Helm Chart**

Create a new Helm chart for deploying the application:

helm create cosmocloud-deploy

This will generate the necessary folder structure and files:

- charts/: Contains any chart dependencies (we don’t need this in this case).
- templates/: This folder contains Kubernetes resource definitions (like Deployments, Services, and ConfigMaps).
- values.yaml: A file that holds configurable values for the chart (e.g., container image names, replicas, environment variables).

**2.2 Modify the Chart.yaml**

In the cosmocloud-deploy folder, modify the Chart.yaml file to describe your Helm chart:

apiVersion: v2

name: cosmocloud-deploy

description: A Helm chart for deploying backend, frontend, and Redis services

version: 0.1.0


**3. Define Kubernetes Resources in the Helm Chart**

In this step, we define the **Deployments** and **Services** for the **Backend**, **Frontend**, and **Redis** services within the templates/ folder.

**3.1 Backend Deployment and Service**

Create a backend-deployment.yaml file and define the backend service with the following settings:

- The backend will run with a single replica.
- Set an environment variable REDIS\_URI for connecting to Redis service.
- The backend service will be exposed internally with a ClusterIP type service on port 8000.

**3.2 Frontend Deployment and Service**

Create a frontend-deployment.yaml file to define the frontend deployment and service:

- The frontend will also run with a single replica.
- Set an environment variable BACKEND\_URL to point to the backend service.
- The frontend service will be exposed externally with a NodePort type on port 5173, and a NodePort 31000.

**3.3 Redis Deployment and Service**

Create a redis-deployment.yaml file to define the Redis service:

- Redis will run with a single replica.
- Redis will be exposed internally with a ClusterIP type service on port 6379.

**4. Configure Values in values.yaml**

Edit the values.yaml file to make your Helm chart configurable. For example:

- **Backend**: 
  - Docker image shreybatra/sample-backend
  - Environment variable REDIS\_URI pointing to redis://redis-svc:6379
- **Frontend**: 
  - Docker image shreybatra/sample-frontend
  - Environment variable BACKEND\_URL pointing to http://backend-svc:8000
- **Redis**: 
  - Docker image redis

This file makes the Helm chart flexible by allowing you to easily adjust configurations like image names and environment variables without modifying the actual Kubernetes YAML files.


**5. Install the Helm Chart**

Now that you’ve defined your Helm chart, you can install it to deploy the application stack to your Minikube cluster.

**5.1 Install the Helm Chart**

Run the following command to install your Helm chart:

helm install testapp ./cosmocloud-deploy --atomic --timeout 30s

This command will deploy the application stack, including the backend, frontend, and Redis components.


**6. Verify the Deployment**

**6.1 Check Resources**

To check if all resources have been deployed successfully, use the following command:

kubectl get all

This will show the status of your Pods, Services, Deployments, etc.

**6.2 Check Services**

To access your frontend application, use the following command to check the services:

kubectl get svc

You will see the frontend-svc service with a NodePort, which can be accessed externally. To get the IP of your Minikube instance:

minikube ip

Then, access the frontend at http://<minikube-ip>:31000.

-----
**7. Debugging and Logs**

If you face any issues, you can check the logs of the pods:

1. Get the pod names:
1. kubectl get pods
1. View logs for a specific pod:
1. kubectl logs <pod-name>

This will help identify issues with the backend, frontend, or Redis services.










