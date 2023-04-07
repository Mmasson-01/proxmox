# Uptime Kuma HELM installation

1. Add the HELM repo

```bash
helm repo add uptime-kuma https://dirsigler.github.io/uptime-kuma-helm
```

2. Upgrade your HELM repo list

```bash
helm repo update
```

3. Install the HELM repo with custom values

```bash
helm install uptime-kume uptime-kuma/uptime-kuma --namespace monitoring --create-namespace -f values.yaml
```


## Volumes & Persistance

If persistance enabled, it is required to create a PersistantVolume before hand.

1. Ensure the volume path exist on the host

```bash
mkdir -p <path>
```

2. Apply the PersistantVolume (PV)

```bash
kubectl apply -f pv.yaml
```
