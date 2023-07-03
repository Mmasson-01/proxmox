# Jellyfin

## Add chart

```bash
helm repo add utkuozdemir https://utkuozdemir.org/helm-charts
```

## Get the chart values

```bash
helm show values utkuozdemir/jellyfin > values.yml
```

## Persistent Volume

First create 2 persistent volume for Config and Data.

```bash
kubectl apply -f pv.yml
```

## Install the chart built-in

```bash
helm install jellyfin utkuozdemir/jellyfin
```

