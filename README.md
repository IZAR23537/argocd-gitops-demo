# GitOps Deployment with ArgoCD

This project demonstrates how to use GitOps principles to manage Kubernetes deployments using ArgoCD.

## What's Included

- Kubernetes manifests for a sample NGINX app
- ArgoCD `Application` resource to sync with this repo
- GitOps-style workflow: ArgoCD automatically applies updates from Git

---

## Prerequisites

- A running Kubernetes cluster (Minikube, Kind, etc.)
- ArgoCD installed (see below)
- `kubectl` and `argocd` CLI tools

---

## Install ArgoCD (Local Cluster)

```bash
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Login:

```bash
argocd login localhost:8080
# Default user: admin
# Get initial password:
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```

---

## Create GitOps App in ArgoCD

Apply this:

```bash
kubectl apply -f argocd/app.yaml
```

This links your Git repo with ArgoCD. Once applied, ArgoCD will sync the manifests and deploy the app.

---

## Project Structure

```
argocd-gitops-demo/
├── manifests/
│   ├── namespace.yaml
│   ├── deployment.yaml
│   └── service.yaml
└── argocd/
    └── app.yaml
```

---

## Clean Up

```bash
argocd app delete nginx-app
kubectl delete -f argocd/app.yaml
```

---
