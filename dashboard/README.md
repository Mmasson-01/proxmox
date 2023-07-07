# Kubernetes dashboard

https://artifacthub.io/packages/helm/k8s-dashboard/kubernetes-dashboard

## Add Repo & Install Chart with values

```bash
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --values dashboard-values.yml
```

### Accessing dashboard

Doc: https://github.com/kubernetes/dashboard/blob/master/docs/user/accessing-dashboard/README.md

```bash
export POD_NAME=$(kubectl get pods -n kube-metrics -l "app.kubernetes.io/name=kubernetes-dashboard,app.kubernetes.io/instance=kubernetes-dashboard" -o json path="{.items[0].metadata.name}")
echo https://127.0.0.1:8443/
kubectl -n kube-metrics port-forward $POD_NAME 8443:8443
```

### Create an RBCA Token

```yml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-metrics
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-metrics
```

```bash
kubectl -n kube-metrics create token admin-user
```

