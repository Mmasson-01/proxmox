# Jellyfin

## Add chart

```bash
helm repo add utkuozdemir https://utkuozdemir.org/helm-charts
```

## Get the chart values

```bash
helm show values utkuozdemir/jellyfin > values.yml
```

## Install the chart built-in

```bash
helm install my-release utkuozdemir/jellyfin
```
