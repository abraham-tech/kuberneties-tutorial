# kuberneties-tutorial
# Kubernetes Tutorial

Welcome to the **Kubernetes Tutorial**! This guide will help you understand and work with Kubernetes, a powerful container orchestration system.

## Introduction to Kubernetes
Kubernetes (often abbreviated as K8s) is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. Originally developed by Google and now maintained by the Cloud Native Computing Foundation (CNCF), Kubernetes is widely used to manage modern cloud-native applications.

## Key Features of Kubernetes
- Container Orchestration – Manages multiple containers efficiently across different machines.
- Automatic Scaling – Adjusts the number of running containers based on traffic or resource usage.
- Load Balancing – Distributes traffic between containers to ensure high availability.
- Self-Healing – Automatically restarts failed containers and replaces unresponsive ones.
- Declarative Configuration – Uses YAML or JSON files to define application states and manage infrastructure as code.
- Service Discovery – Provides internal networking and service communication without needing manual IP configuration.
## Basic Kubernetes Components
- Pod: The smallest deployable unit in Kubernetes, containing one or more containers.
- Node: A machine (virtual or physical) that runs Pods.
- Cluster: A group of nodes managed by Kubernetes.
- Deployment: Manages replicas of Pods for scaling and updates.
- Service: Provides a stable network endpoint for accessing Pods.
- Ingress: Manages external access to services, typically via HTTP/HTTPS.
- ConfigMap & Secret: Stores configuration data and sensitive information separately from application code.

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

We've supplied two verisons of the yaml for monitoring - for EKS and Kops.

What's the difference between the two?

At the time of writing (November 2021), actually there is no difference between the two. We previously needed to maintain separate versions.

However, I've recently made changes to the yaml which now means there's no difference. I'm keeping the files separate so that the videos don't need to be re-recorded!

The changes were made due to changes in the latest versions of Kubernetes, and I wanted to make it so there was no impact on the course videos.

BUT:

If you are planning to deploy to a production *Kops* cluster (EKS is fine), please note that the version shipped here has removed several control plane alerts. I needed to remove these alerts because Kops users would get spurious control plane (master) alerts which can only be silenced via an awkward workaround (see below).

So you could install my kops-monitoring.yaml to your cluster, but there's a chance you might be missing out on some valuable alerts. So I recommend for a production cluster, get the latest version of the monitoring stack (with all alerts enabled) from https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack

The section in the course on "Helm" explains how to generate the yaml from their Helm chart.

Then you would need to make a change to your cluster as follows:

kops edit cluster

Then insert the following two lines, as a child element of "spec:"

spec:
.
.
  kubeProxy:
    metricsBindAddress: 0.0.0.0

After doing this, you will need to run "kops update cluster --yes" and then run "kops rolling-update cluster". This will terminate all the nodes in your cluster one by one (so the only one node is unavailable at any one time), and will make the changes needed.

Full details of the reason behind this (it is quite obscure) here: https://github.com/helm/charts/issues/16476

Without doing this, you will get lots of spurious alerts about the control plane.
