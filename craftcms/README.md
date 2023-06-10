# CraftCMS

## Create the Mysql statefulset first

### Create Persistent Volume

```bash
ssh user@k8smaster
sudo mkdir -p /data/volumes/mysql-pv
kubectl apply -f db-persistant-volume.yaml
```

### Create Database initial secrets

```bash
kubectl apply -f ./db-configmap.yaml
```


### Create Mysql statefulset + service

```bash
kubectl apply -f ./db-statefulset.yaml
kubectl apply -f ./db-service.yaml
```

## Create CraftCMS deployment


### Create CraftCMS initial secrets

```bash
kubectl apply -f ./craft-configmap.yaml
```

### Create CraftCMS deployment and ingress

```bash
kubectl apply -f ./craft-deployment.yaml
kubectl apply -f ./craft-service.yaml
kubectl apply -f ./craft-ingress.yaml
```

## Setup CraftCMS (if using the craftcms empty image)

- storage directory is missing
- database is empty, need to install craftcms first

```bash
kubectl exec -it craft-deployment-<id> -- sh
mkdir storage
php craft setup/welcome
```

## Update hosts file or deploy cloudflared tunnel

```bash
sudo vi /etc/hosts
<id> craft.starstruckcreative.com
```

```bash
cloudflared tunnel --origincert=tunnel.pem route dns starstruckcreative craft.starstruckcreative.com
```

Enjoy !

https://craft.starstruckcreative.com
