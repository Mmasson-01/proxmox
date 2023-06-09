# Sonarqube

## Add helm repo

```bash
helm repo add sonarqube https://SonarSource.github.io/helm-chart-sonarqube
```

## Create namespace

```bash
kubectl create namespace sonarqube
```

## Create PV

```bash
kubectl apply -f pv.yaml
```
### Volumes & Persistance

If persistance enabled, it is required to create a PersistantVolume before hand.

1. Ensure the volume path exist on the host

```bash
mkdir -p /data/volumes/sonarqube-pv
```


## Install helm chart with custom values

```bash
helm upgrade --install -n sonarqube sonarqube -f values.yaml sonarqube/sonarqube
```


# Add DNS to cloudflared

```bash
cloudflared tunnel --origincert=tunnel.pem route dns starstruckcreative sonarqube.starstruckcreative.com
```
