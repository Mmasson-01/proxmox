# Helm chart ingress-nginx

## Documentation
https://kubernetes.github.io/ingress-nginx/deploy/


## Install chart

```bash
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```
