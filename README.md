# kuberneties-tutorial
# Kubernetes Tutorial

Welcome to the **Kubernetes Tutorial**! This guide will help you understand and work with Kubernetes, a powerful container orchestration system.

## Prerequisites

Before starting, ensure you have the following installed:
- Docker
- Kubernetes (kubectl CLI)
- Minikube (for local Kubernetes cluster)
- Helm (for package management)

## Getting Started

### 1. Start Minikube
```sh
minikube start
```

### 2. Check Cluster Status
```sh
kubectl cluster-info
kubectl get nodes
```

### 3. Deploy an Application
```sh
kubectl create deployment my-app --image=nginx
```

### 4. Expose the Application
```sh
kubectl expose deployment my-app --type=NodePort --port=80
```

### 5. Get Service Details
```sh
kubectl get services
```

### 6. Access the Application
```sh
minikube service my-app
```

## Important Kubernetes Commands

### Cluster Management
```sh
kubectl config view          # View Kubernetes configuration
kubectl get nodes            # List cluster nodes
kubectl get pods --all-namespaces # List all pods
```

### Deployments and Pods
```sh
kubectl create deployment my-app --image=nginx  # Create a deployment
kubectl get deployments                        # List deployments
kubectl get pods                               # List running pods
kubectl describe pod <pod-name>                # Get pod details
kubectl delete pod <pod-name>                  # Delete a pod
```

### Services and Networking
```sh
kubectl get services           # List services
kubectl expose deployment my-app --type=NodePort --port=80  # Expose app
kubectl port-forward pod/<pod-name> 8080:80  # Forward port
```

### Scaling and Updates
```sh
kubectl scale deployment my-app --replicas=3  # Scale deployment
kubectl set image deployment/my-app nginx=nginx:1.19  # Update image
```

### ConfigMaps and Secrets
```sh
kubectl create configmap my-config --from-literal=key=value  # Create ConfigMap
kubectl get configmaps                                      # List ConfigMaps
kubectl create secret generic my-secret --from-literal=password=12345  # Create Secret
kubectl get secrets                                          # List Secrets
```

### Cleanup
```sh
kubectl delete deployment my-app  # Delete a deployment
kubectl delete service my-app      # Delete a service
kubectl delete pod <pod-name>      # Delete a pod
minikube stop                      # Stop Minikube
```

## Conclusion

This tutorial covered basic Kubernetes commands for managing deployments, services, and configurations. For more advanced topics, check out the [official Kubernetes documentation](https://kubernetes.io/docs/).

