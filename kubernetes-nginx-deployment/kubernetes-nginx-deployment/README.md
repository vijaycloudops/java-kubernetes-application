# Simple Kubernetes Nginx Deployment

A beginner-friendly Kubernetes project that deploys an Nginx web server using Kubernetes manifests. It demonstrates namespaces, Deployments, Pods, Services, labels, resource requests/limits, and health probes.

## What this project creates

- A Kubernetes namespace named `nginx-demo`
- A Deployment with 2 Nginx replicas
- A NodePort Service to expose Nginx
- CPU and memory requests/limits
- Readiness and liveness probes

## Architecture

```text
Your browser
    |
    | http://localhost:30080
    v
NodePort Service (nginx-service)
    |
    v
Deployment (nginx-deployment)
    |
    +---- Pod 1: Nginx container
    |
    +---- Pod 2: Nginx container
```

## Prerequisites (Windows 11)

Choose one local Kubernetes environment:

- Docker Desktop with Kubernetes enabled, or
- Minikube with kubectl installed.

Check that kubectl can reach your cluster:

```powershell
kubectl version --client
kubectl get nodes
```

## Project structure

```text
kubernetes-nginx-deployment/
├── k8s/
│   ├── namespace.yaml
│   ├── deployment.yaml
│   └── service.yaml
├── .gitignore
└── README.md
```

## Deploy the application

Run these commands from the project root in PowerShell.

### 1. Create the namespace

```powershell
kubectl apply -f k8s/namespace.yaml
```

### 2. Deploy Nginx

```powershell
kubectl apply -f k8s/deployment.yaml
```

### 3. Expose Nginx using a NodePort Service

```powershell
kubectl apply -f k8s/service.yaml
```

### 4. Check Kubernetes resources

```powershell
kubectl get namespaces
kubectl get deployments -n nginx-demo
kubectl get pods -n nginx-demo
kubectl get services -n nginx-demo
```

Wait until both Pods show `Running` and `READY` as `1/1`.

### 5. Open the application

For Docker Desktop Kubernetes, open:

```text
http://localhost:30080
```

For Minikube, run:

```powershell
minikube service nginx-service -n nginx-demo
```

If the NodePort URL does not open in your environment, use port-forwarding:

```powershell
kubectl port-forward service/nginx-service 8080:80 -n nginx-demo
```

Then open `http://localhost:8080`.

## Useful troubleshooting commands

```powershell
kubectl describe pods -n nginx-demo
kubectl logs deployment/nginx-deployment -n nginx-demo
kubectl get events -n nginx-demo --sort-by=.metadata.creationTimestamp
```

## Update and scale

Change the replica count:

```powershell
kubectl scale deployment nginx-deployment --replicas=3 -n nginx-demo
kubectl get pods -n nginx-demo
```

Roll out the latest declared configuration:

```powershell
kubectl rollout status deployment/nginx-deployment -n nginx-demo
kubectl rollout history deployment/nginx-deployment -n nginx-demo
```

## Clean up

Delete all project resources:

```powershell
kubectl delete namespace nginx-demo
```

## Interview explanation

"I created a simple Kubernetes application deployment using YAML manifests. I created a namespace to isolate the project resources, a Deployment to maintain two Nginx replicas, and a NodePort Service to expose the application. I configured resource requests and limits along with readiness and liveness probes. I used kubectl to deploy, inspect, scale, troubleshoot, and clean up the application."

## Concepts demonstrated

| Kubernetes concept | Purpose |
|---|---|
| Namespace | Logical isolation for project resources |
| Pod | Runs the Nginx container |
| Deployment | Maintains the desired number of Pods |
| Replica count | Runs two application replicas |
| Service | Provides stable access to Pods |
| NodePort | Exposes the Service on a node port |
| Requests and limits | Define container resource requirements and caps |
| Readiness probe | Checks whether the container is ready for traffic |
| Liveness probe | Checks whether the container should be restarted |

## Next improvements

- Build a custom Docker image and deploy it.
- Add ConfigMaps and Secrets.
- Add Ingress with a local ingress controller.
- Package the application using Helm.
- Add CI/CD using GitHub Actions.
- Deploy the same manifests to a managed Kubernetes service such as AKS or EKS.

This is a learning project. It uses the public Nginx image and is not intended as a production deployment.
