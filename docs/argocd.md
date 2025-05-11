## ArgoCD

Getting started: [https://argo-cd.readthedocs.io/en/stable/getting_started/](https://argo-cd.readthedocs.io/en/stable/getting_started/)

Create namespace:
```bash
kubectl create namespace argocd
```

Install:
```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Ingress:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: argocd-ingress
  namespace: argocd
  annotations:
    kubernetes.io/ingress.class: "nginx"
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  ingressClassName: "nginx"
  rules:
  - host: argocd.ecommerce.local # hosts file entry pointing to WSL IP required
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: argocd-server
            port:
              number: 443
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

Configure ArgoCD: `argocd-config.yaml`
```yaml
# run: `kubectl api-resources | grep argo` to see API resources available and if 'v1alpha' is correct
# also see this sample app: https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#applications
apiVersion: argoproj.io/v1alpha1 
kind: Application
metadata:
    name: ecommerce-argocd
    namespace: argocd
spec:
  project: default # every application belongs to a project, default project/value is 'default'
  source:
    repoURL: https://github.com/mehmetgunacti/ecommerce-parent.git
    targetRevision: HEAD
    path: k8s
  destination:
    server: https://kubernetes.default.svc
    namespace: ecommerce
  syncPolicy:
    syncOptions:
    - CreateNamespace=true
    automated:
      selfHeal: true
      prune: true
```

Ingress:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: argocd-ingress
  namespace: argocd
  annotations:
    kubernetes.io/ingress.class: "nginx"
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  ingressClassName: "nginx"
  rules:
  - host: argocd.ecommerce.local # hosts file entry pointing to WSL IP required
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: argocd-server
            port:
              number: 443
```