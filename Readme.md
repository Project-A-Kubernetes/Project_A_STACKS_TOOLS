# Cluster Stack Deployment with Argo CD
### Overview

This repository demonstrates the deployment of the Prometheus monitoring stack, ingress-nginx-controller, certificate-manager etc on a Kubernetes cluster using Helm and Argo CD. The setup includes GitOps-based management, persistent storage configuration, and automated application syncing.
## Simple Deployment diagram 
![Architecture Diagram](image.png)
## Repository Structure
```
prometheus-stacks/
├── Readme.md
|___ controller/
|   |___ ingress-nginx-controller.yml
|   |___ cert-manager.yml
|
├── stacks/
│   ├── stack-argocd.yml   # Argo CD Application manifest
│   └── value.yml          # Helm chart values
└── storage/
    ├── argocd-watch.yml
    └── storageclass.yml
```

### Steps Completed
1. Argo CD Installation

    - Installed Argo CD via Terraform.
2. GitHub App Integration

    - Configured a GitHub App for Argo CD repository access.
    - Obtained GitHub App ID and Installation ID from the organization.
    - Added the repository to Argo CD using:
```
argocd repo add https://github.com/Project-A-Kubernetes/Project_A_STACKS_TOOLS.git \
  --github-app-id APP_ID \
  --github-app-installation-id INSTALLATION_ID \
  --github-app-private-key-path PRIVATE_KEY_PATH
```
This connected my argocd repo server to my github repo (over HTTPS) as the only source of truth to the cluster 

3. Prometheus Helm Chart Configuration

    - Used the kube-prometheus-stack Helm chart from the Prometheus Community Helm repository.

4. Persistent Storage Configuration

    - Configured StorageClass (gp3) for Prometheus persistent volumes.
    - Ensured PVs and PVCs are properly bound.
    - Resolved pod issues caused by PVs stuck in the Deleting state.
5. Ingress-nginx-controllers
    - Used the official helm chart to configure ingress-nginx to the cluster 
6. Cert-manager 
    - Used the offical helm repository to install  the helm repo to the cluster 

## How to run
After applying the terraform repository network,Eks,platform and other resources running, then you apply this to the cluster by adding this repository url to the "argocd repo add "  and apply the argocd Application objects to the cluster
example :
```
# please clone the repo and create your github-app from settings/github-app
Then
  argocd app create -f <app-name>
Then
argocd repo add https://github.com/Project-A-Kubernetes/Project_A_STACKS_TOOLS.git \
  --github-app-id APP_ID \
  --github-app-installation-id INSTALLATION_ID \
  --github-app-private-key-path PRIVATE_KEY_PATH

```
#By doing this any other future configuration will be automated just with any change to the repository
