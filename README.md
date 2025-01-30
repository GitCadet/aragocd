# 🦑 ArgoCD on Kubernetes

**Project Overlook:** Deploying a ArgoCD agent in my Kubernetes cluster to automate application deployment using GitOps principles.                                               
**NOTE: I used pw in exchange of my actual password in the README*
## Prerequisites

Before I started the setup, I made sure I had the following:
- A running Kubernetes cluster.
- "`kubectl`" configured to access my Kubernetes cluster.
- A GitLab repository containing my Kubernetes manifests.

## Setting Up ArgoCD

### Creating the ArgoCD Namespace:
To set up ArgoCD, I followed these steps:
1. I created a dedicated namespace for ArgoCD by running "`kubectl create namespace argocd`".
2. Then, I applied the necessary ArgoCD YAML configuration "`kubectl apply -n argocd -f`" -> ArgoCD Config URL.yaml.
3. I verified the ArgoCD pods were created successfully by running "`kubectl get pods -n argocd`".

### Accessing the ArgoCD UI:
To access the ArgoCD UI, I followed these steps:
1. I listed the available services in the argocd namespace by running;
   - "`kubectl get svc -n argocd`"
2. I forwarded the "`argocd-server`" service to make it accessible locally using;
   - "`kubectl port-forward svc/argocd-server -n argocd 8080:443`"
3. Then, I opened "`http://localhost:8080`" in my browser. This displayed the login page.
4. I logged in using the default admin user and retrieved the initial password with this command;
   - "`kubectl get secret argocd-initial-admin-secret -n argocd -o yaml`"
   - Then used "`echo pw | base64 --decode`" to display the password in plain text.

## Configuring ArgoCD

### Connecting ArgoCD to GitLab:

To connect to GitLab repo, I created an "Application" Custom Resource Definition (CRD). The configuration I used included:

- **Source**: The Git repository URL that contains my Kubernetes manifests.
- **Path**: The `dev` directory in the repo containing the deployment configurations.
- **Destination**: The Kubernetes cluster I wanted my app to be deployed in.

### Syncing and Automation:

To automate the synchronisation process, I configured ArgoCD with the following settings:

- *"syncPolicy"* attribute to enable automatic synchronisation.
- *"automated"* attribute with the options:
  - "`selfHeal: true`" - To override manual changes and sync changes from the Git repo.
  - "`prune: true`" - To delete resources that are no longer needed in the Cluster.

## Deploying my Application

1. I pushed my "`application.yaml`" file to my remote Git repository.
2. Then, I applied the configuration to ArgoCD using "`kubectl apply -f application.yaml`".
3. I verified the application status via the ArgoCD UI to ensure it was healthy and in sync.

![image_alt](https://github.com/GitCadet/aragocd/blob/main/my-app-argocd-ui.png?raw=true)

## Testing Sync by Updating Deployment

To test the synchronisation process, I updated the "`deployment.yaml`" file in my GitLab repository to use a new version of my Docker image. ArgoCD automatically detected the change and applied it, confirming that the sync feature was working correctly.

## Overview

This project helped me in:
- Understanding key ArgoCD and Kubernetes concepts.
- Automating deployment using GitOps principles.
- Building configuration files for the ArgoCD agent.
