## ArgoCD

Getting started: [https://argo-cd.readthedocs.io/en/stable/getting_started/](https://argo-cd.readthedocs.io/en/stable/getting_started/)

Create namespace:
```bash
kubectl create namespace argocd
```

Install ArgoCD:
```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Configure ArgoCD and Ingress
```bash
kubectl apply -f argocd.yaml
```

For `admin` password:
```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
# Output: 'data'->'password': cWh5TUU3RGNKcUZqUGNJZg==

# Decode:
echo cWh5TUU3RGNKcUZqUGNJZg== | base64 --decode
# Output: qhyME7DcJqFjPcIf
# in case there's a % sign at the end, ignore it
```