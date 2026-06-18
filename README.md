# K8s DevOps Project

## Stack
- **App**: nginxdemos/hello (public demo image)
- **Database**: Redis 7 (alpine)
- **Cluster**: Minikube (local Kubernetes)
- **CI/CD**: GitHub Actions
- **Reliability**: Liveness + Readiness Probes
- **Failure Sim**: Bad image tag simulation

## Project Structure
k8s/
├── deployment.yaml   # Main app deployment with probes
├── redis.yaml        # Redis database + service
└── service.yaml      # Exposes app via NodePort

## How to Run Locally

### Prerequisites
- Minikube
- kubectl
- Docker Desktop

### Start Minikube
minikube start --driver=docker

### Deploy Everything
kubectl apply -f k8s/

### Access App
minikube service myapp-service --url

### Check Status
kubectl get all

## Reliability Choice: Readiness + Liveness Probes
- **Liveness Probe**: Restarts container if app crashes
- **Readiness Probe**: Removes pod from load balancer if not ready
- Together they ensure zero downtime and no traffic to broken pods

## Failure Simulation
Injected bad image tag to simulate failed deployment:
kubectl set image deployment/myapp myapp=nginxdemos/hello:broken
- Observed: ImagePullBackOff error
- Debugged: kubectl describe + kubectl get events
- Fixed: kubectl set image deployment/myapp myapp=nginxdemos/hello
