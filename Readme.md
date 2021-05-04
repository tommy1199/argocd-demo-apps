# Demo Repository for showcasing GitOps with ArgoCD

## Setup cluster

```bash
kind create cluster --config=cluster.yaml
```

## Install ArgoCD and ApplicationSet

```bash
kubectl create ns argocd
kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml -n argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj-labs/applicationset/v0.1.0/manifests/install.yaml
```

Patch ArgoCD service to be available externally via 31000

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort", "ports": [{"name": "https", "nodePort": 31000, "port": 443}]}}'
```

## Install Sealed Secrets Operator

```bash
kubectl apply -n kube-system -f sealed-secrets/secret.yaml
kubectl apply -f https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.15.0/controller.yaml
```

## Login to ArgoCD

Retrieve password:

```bash
ARGOCD_PASSWORD=$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)
```

Open ArgoCD in browser

```bash
open https://localhost:31000
```

Login with credentials:

User: admin
Password: $ARGOCD_PASSWORD
