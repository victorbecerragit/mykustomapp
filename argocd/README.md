
# Patch service argocd-server

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'

# Port-forward argocd-server

kubectl port-forward svc/argocd-server -n argocd 8080:443

# Get secret 

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

# Install argocd CLI

curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64


# Login by CLI

argocd login localhost:8080

# Deploy webapp from this repo

argocd app create helm-webapp --repo https://github.com/victorbecerragit/mykustomapp.git --path helm-chart --dest-server https://kubernetes.default.svc --dest-namespace default
