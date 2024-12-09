# Cosmocloud task submission explanation and deployment steps

Greetings to the cosmocloud team, I am **Aditya Zaveri** and this is my submission for the task round. I am a resident of Noida, UP and had prior experience to AWS but not Docker and Kubernetes. This task round gave me exposure on learning and debugging and through this Readme, I will navigate on how to deploy and test this app, including why some steps have been implemented.

## Project Overview: Containerized Microservices

This project implements a **containerized microservices** architecture with three services, each running in it's own container to ensure isolation and ease of deployment.

**1.	Backend Service**
The Backend service, built using the shreybatra/sample-backend image, handles business logic and API requests from the frontend. It processes data and communicates with other services as needed.

**2. Frontend Service**
The Frontend service, using the shreybatra/sample-frontend image, is responsible for rendering the user interface and interacting with the backend via API calls.

**3.	Redis Service**
The Redis service, using the official redis image.

**About helm charts**

A Helm chart is a package for bundling all the necessary resources to deploy an application on a Kubernetes cluster. It consists of collection of files that define and configure the application’s resources,enabling deployment.

Helm charts consist of three components:

1.	Chart.yaml: Contains metadata about the application, such as its name, version, and any dependencies.
 
2.	Values.yaml: Specifies values that allow for chart customization.
 
3.	Templates Directory: Houses template files, which are combined with values from the values.yaml file to generate Kubernetes manifests.

Helm charts enable the ability to be reused across various environments, simplifying configuration management. Each chart can be versioned and maintained independently, helping to streamline the process of managing multiple application versions.

## Technical Overview:

**•	Containerization:** Each service is containerized with Docker, providing portability and consistency across environments.

**•	Kubernetes Orchestration:** The services are managed and scaled using Kubernetes, and minikube.

## Application Stack

•	Backend communicates with Redis using the environment variable REDIS_URI set to **redis://redis-svc:6379.**

•	Frontend communicates with Backend using the environment variable BACKEND_URL set to **http://backend-svc:8000.**

## Services:

**Backend Service:**

	•	Type: ClusterIP
	•	Port: 8000
	•	Name: backend-svc

**Frontend Service:**

	•	Type: NodePort
	•	Port: 5173
	•	NodePort: 31000
	•	Name: frontend-svc

**Redis Service:**

	•	Type: ClusterIP
	•	Port: 6379
	•	Name: redis-svc

## Prerequisites

•	**Minikube:** local Kubernetes environment.

•	**Helm:** Kubernetes package manager for deploying applications.

•	**Kubectl:** Kubernetes command-line tool.

## Project Structure

**The Helm chart** is located in the cosmocloud-deploy directory and contains the following files:

•	**Chart.yaml:** Contains metadata about the Helm chart.

•	**values.yaml:** Defines default values for environment variables and other configuration.

•	**templates/deployment-backend.yaml:** Kubernetes deployment for the backend service.

•	**templates/deployment-frontend.yaml:** Kubernetes deployment for the frontend service.

•	**templates/deployment-redis.yaml:** Kubernetes deployment for the Redis service.

•	**templates/service-backend.yaml:** Kubernetes service for the backend.

•	**templates/service-frontend.yaml:** Kubernetes service for the frontend.

•	**templates/service-redis.yaml:** Kubernetes service for Redis.

<img width="229" alt="Screenshot 2024-12-09 at 16 37 11" src="https://github.com/user-attachments/assets/72aa782c-9888-4ff7-9023-ed0b9b89e52c">

## Steps for Deployment

We can do this by using docker-desktop kubernetes or by minikube

**a.Docker Desktop kubernetes**

```bash
  helm install testapp cosmocloud-deploy --atomic --timeout 30s
```

and app will be visible at localhost:31000

**or**

**b)Using Minikube**

**1. Start Minikube**

Start a local Kubernetes cluster with Minikube, run the following command:

```bash
minikube start
```
**2. Set Up Helm**

Ensure Helm is installed.

**3. Create Helm Chart**

create the Helm chart (after cloning repository):

```bash 
helm install testapp cosmocloud-deploy --atomic --timeout 30s

```

**4. Port Forwarding**

Access the frontend from your local machine, use port forwarding:

```bash
kubectl port-forward service/frontend-svc 31000:5173
```

After this, the front end will be accessible at at http://localhost:31000.

**5. Verify Deployment(optional)**

Verify the deployment:
```bash
kubectl get deployments
kubectl get services
```
 Ensure the output has :

	•	One backend deployment.
	•	One frontend deployment.
	•	One Redis deployment.
	•	Three services: backend-svc, frontend-svc, and redis-svc.
<img width="921" alt="Screenshot 2024-12-06 at 17 08 48" src="https://github.com/user-attachments/assets/83b0d11e-5bcf-473f-bf6f-c53838988d9b">

## Explanation and Debugging

After running the commands:

1.Minikube starts a local Kubernetes cluster for hosting the app.

2.Helm deploys the backend, frontend, and Redis as containers with Kubernetes managing their configuration and communication.

3.Port forwarding makes the frontend accessible locally via http://localhost:31000.

4.The app becomes fully operational, with the frontend communicating with the backend and Redis running.

The problem i faced to verify my deployment was of accessing the frontend part of the deployment. I had tried all ways for accessing the frontend on my local machine,such as using minikube ip and verifying all services were running and correct mappings were used, and that my backend was servicing the frontend, but all was unsuccesful. After researching about this issue, I came up with two solutions that work the best:-
( Source for resolving issue:- https://stackoverflow.com/questions/71536310/unable-to-access-minikube-ip-address)

**1. Use the command**

```bash
minikube service frontend-svc
```
This automatically opens the frontend on your local system.
<img width="909" alt="Screenshot 2024-12-06 at 17 07 23" src="https://github.com/user-attachments/assets/42f36bf4-402e-4429-a404-0f2383362a7b">

**2. Port Forwarding**

```bash
kubectl port-forward service/frontend-svc 31000:5173
```
This forwards traffic from local machine’s port 31000 to the frontend-svc service on port 5173 inside the Kubernetes cluster.
<img width="921" alt="Screenshot 2024-12-06 at 17 07 55" src="https://github.com/user-attachments/assets/3a33d99f-085a-4ca8-8a43-fa586547bc91">

This behaviour is caused by the inherent working of minikube, as it creates its own local network inside the container, which is isolated from the outside world. This isolation helps prevent potential conflicts between the Minikube Kubernetes cluster and host's operating system’s network interfaces.

## Conclusion

This Deploys a fully functional application stack with backend, frontend, and Redis services in a Kubernetes cluster. I hope to hear from the cosmocloud team soon. 

Deployment screenshot:
<img width="1582" alt="Screenshot 2024-12-06 at 16 08 32" src="https://github.com/user-attachments/assets/586ec0e5-cecf-4ec8-839f-e17a06dc8847">


