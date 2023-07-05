# Metric Server Helm Chart

## Add Repo

```bash
helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/
```

## Install Chart with new values

```bash
helm upgrade --install metrics-server metrics-server/metrics-server --values values.yml
```


### What is changed?

Only the default arg `--kubelet-insecure-tls` was added.

```yaml
defaultArgs:
  - --cert-dir=/tmp
  - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
  - --kubelet-use-node-status-port
  - --metric-resolution=15s
  - --kubelet-insecure-tls
```
