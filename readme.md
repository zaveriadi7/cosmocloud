# Cosmocloud task submission explanation and deployment steps

Greetings to the cosmocloud team, I am **Aditya Zaveri** and this is my submission for the task round. I am a resident of Noida, UP and had prior experience to AWS but not Docker and Kubernetes. This task round gave me exposure on learning and debugging and through this Readme, I will navigate on how to deploy and test this app, including why some steps have been implemented.

## Project Overview: Containerized Microservices

This project implements a **containerized microservices** architecture with three services, each running in its own container to ensure isolation and ease of deployment.

**1.	Backend Service**
The Backend service, built using the shreybatra/sample-backend image, handles business logic and API requests from the frontend. It processes data and communicates with other services as needed.

**2. Frontend Service**
The Frontend service, using the shreybatra/sample-frontend image, is responsible for rendering the user interface and interacting with the backend via API calls.

**3.	Redis Service**
The Redis service, using the official redis image.

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

•	**values.yaml:** Defines default values for 	environment variables and other configuration.

•	**templates/deployment-backend.yaml:** Kubernetes deployment for the backend service.

•	**templates/deployment-frontend.yaml:** Kubernetes deployment for the frontend service.

•	**templates/deployment-redis.yaml:** Kubernetes deployment for the Redis service.

•	**templates/service-backend.yaml:** Kubernetes service for the backend.

•	**templates/service-frontend.yaml:** Kubernetes service for the frontend.

•	**templates/service-redis.yaml:** Kubernetes service for Redis.


## Steps for Deployment

**1. Start Minikube**

To start a local Kubernetes cluster with Minikube, run the following command:

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

To access the frontend from your local machine, use port forwarding:

```bash
kubectl port-forward service/frontend-svc 31000:5173
```

After this, the frontend will be accesible at at http://localhost:31000.

**5. Verify Deployment(optional)**

Check if the deployment is successful:
```bash
kubectl get deployments
kubectl get services
```
You should see the following:

	•	One backend deployment.
	•	One frontend deployment.
	•	One Redis deployment.
	•	Three services: backend-svc, frontend-svc, and redis-svc.

## Explanation

The problem i faced to verify my deployment was of accessing the frontend part of the deployment. I had tried all ways for accessing the frontend on my local machine,such as using minikube ip and verifying all services were running and correct mappings were used, and that my backend was servicing the frontend, but all was unsuccesful. After researching about this issue, i came up with two solutions that work the best:-
	
**1. Use the command**

```bash
minikube service frontend-svc
```
This automatically opens the frontend on your local system.

**2. Port Forwarding**

```bash
kubectl port-forward service/frontend-svc 31000:5173
```
This forwards traffic from local machine’s port 31000 to the frontend-svc service on port 5173 inside the Kubernetes cluster.

This behaviour is caused by the inherent working of minikube, as it creates its own local network inside the container, which is isolated from the outside world. This isolation helps prevent potential conflicts between the Minikube Kubernetes cluster and host's operating system’s network interfaces.

## Conclusion

This Deploys a fully functional application stack with backend, frontend, and Redis services in a Kubernetes cluster. I hope to hear from the cosmocloud team soon. 

Deployment screenshot:
	https://drive.google.com/file/d/1Hq4km8-6s8kz375v71yfkfNKGksjaJ8v/view?usp=sharing


