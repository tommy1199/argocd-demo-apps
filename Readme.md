# Demo Repository for showcasing GitOps with ArgoCD

## Setup cluster

```bash
kind create cluster --config=cluster.yaml
```

## Install ArgoCD, ApplicationSet and SealedSecrets Operator

```bash
kustomize build | kubectl apply -f -
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
