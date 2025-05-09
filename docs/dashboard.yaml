## Kubernetes Dashboard

Install using `helm`:

```bash
helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --namespace kubernetes-dashboard --create-namespace
```

Apply `ecommerce-dashboard.yaml`:
```bash
kubectl apply -f ecommerce-dashboard.yaml
```

Check if the service account now has access:
```bash
kubectl auth can-i '*' '*' --as=system:serviceaccount:kubernetes-dashboard:dashboard-admin-sa
// Command output should be: yes
```

Map WSL IP in Windows `hosts` file:
```bash
172.26.29.111    dashboard.ecommerce.local
```

Access dashboard:
```bash
https://dashboard.ecommerce.local
```

Generate access token:
```bash
kubectl -n kubernetes-dashboard create token dashboard-admin-sa
```

For convenience add following line to `.bashrc`:
```bash
alias create-token="kubectl -n kubernetes-dashboard create token dashboard-admin-sa"
```