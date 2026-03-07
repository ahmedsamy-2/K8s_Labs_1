# Lab 3: Multi-Tenancy with Namespaces and Internal Routing

## Scenario
Your team is adopting a "cluster per environment" strategy, but for cost reasons, you must use a single cluster for Development (dev) and Staging (staging) environments. You need to ensure logical separation and enable communication between two applications within the same namespace, while keeping environments completely isolated from each other.

## Lab Description
The student will create two namespaces (dev and staging). They will deploy a two-tier application (a frontend and a backend) in the dev namespace and a separate instance in the staging namespace. They will use services for internal pod networking and demonstrate environment isolation.

---
## Build Instructions for Students

The students will need to build these Docker images before deploying to Kubernetes. Provide them with these build commands:

```bash
# Build backend image
docker build -f Dockerfile.backend -t backend-app:latest .

# Build frontend image
docker build -f Dockerfile.frontend -t frontend-app:latest .

# If using minikube, load the images
minikube image load backend-app:latest
minikube image load frontend-app:latest

--- 

## Tasks to Implement

1. Create two namespaces: `dev` and `staging`.

2. In the `dev` namespace, deploy a backend pod (a simple Python HTTP server or redis) and expose it via a ClusterIP service named `backend-service`.

3. In the `dev` namespace, deploy a frontend pod (nginx). Configure it so that it can communicate with the backend pod using the service name `backend-service`.

4. Repeat steps 2 and 3 in the `staging` namespace, ensuring the frontend there communicates with its own local `backend-service`.

5. Verify pod networking by demonstrating that the frontend in `dev` cannot reach the backend service in `staging` by its fully qualified domain name (e.g., `backend-service.staging.svc.cluster.local`).

6. Use imperative commands to temporarily run a debug pod in the `dev` namespace and test DNS resolution and connectivity to both services.

---

## Notes for Students

- The backend server runs on port `8080` and provides three endpoints: `/`, `/health`, and `/info`

- The frontend nginx is configured to proxy requests to the backend using the service name `backend-service`

- The HTML interface provides buttons to test connectivity to different backend endpoints

- Both applications display their pod name and namespace for easy identification when testing environment isolation