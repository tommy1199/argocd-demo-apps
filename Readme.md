kind create cluster --config=cluster.yaml

kubectl create ns argocd

kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml -n argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj-labs/applicationset/v0.1.0/manifests/install.yaml

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort", "ports": [{"name": "https", "nodePort": 31000, "port": 443}]}}'

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

kubectl apply -n kube-system -f sealed-secrets/secret.yaml

kubectl apply -f https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.15.0/controller.yaml
