# HELM install


1. Add the HELM repo

```bash
helm repo add jetstack https://charts.jetstack.io
```

2. Update your HELM cache

```bash
helm repo update
```

3. Install the helm repo

```bash
helm install \                                                          ✔  kubernetes-admin@kubernetes/cert-manager ⎈  07:35:26  ▓▒░
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.11.0 \
  --set installCRDs=true \
  --set prometheus.enabled=false
```

## Verify your installation

1. Check if the following pods are running in a healthy state

```
cert-manager
cert-manager-cainjector
cert-manager-webhook

kubectl get pods --namespace cert-manager
```

2. Create a test resource to verify certificate are issued

```yaml
# test-resource.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: cert-manager-test
---
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: test-selfsigned
  namespace: cert-manager-test
spec:
  selfSigned: {}
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: selfsigned-cert
  namespace: cert-manager-test
spec:
  dnsNames:
    - example.com # Put your real domain
  secretName: selfsigned-cert-tls
  issuerRef:
    name: test-selfsigned
```

```bash
k apply -f test-resource.yaml
kubectl describe certificate -n cert-manager-test # Certificate issued successfully
k delete -f test-resource.yaml
```
