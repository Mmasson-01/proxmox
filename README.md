# Installing Kubernetes on Proxmox debian

## Requirements

```bash
sudo fdisk /dev/sdb
n
p
1 #num partition
enter
enter
w
sudo mkfs.ext4 /dev/sdb1
```


## Install Kubernetes

```bash
sudo apt-get install -y apt-transport-https ca-certificates curl vim lsb-release gnupg fdisk sudo ebtables ethool
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y cri-tools kubelet=1.27.3-00 kubeadm=1.27.3-00 kubectl=1.27.3-00
sudo apt-mark hold kubelet kubeadm kubectl
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo groupadd docker
sudo usermod -aG docker $USER
mkdir -p /etc/modules-load.d
mkdir -p /etc/sysctl.d/
containerd config default |  tee /etc/containerd/config.toml

curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

cat <<EOF | sudo tee /etc/crictl.yaml
runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 2
debug: false
pull-image-on-create: false
EOF

modprobe overlay
modprobe br_netfilter
sysctl --system
lsmod | grep br_netfilter
lsmod | grep overlay
sysctl net.bridge.bridge-nf-call-iptables net.bridge.bridge-nf-call-ip6tables net.ipv4.ip_forward

sed -i 's/SystemdCgroup = false/SystemdCgroup = true/g' /etc/containerd/config.toml
systemctl enable containerd
systemctl restart containerd
kubeadm init --cri-socket /var/run/containerd/containerd.sock --service-cidr 10.0.0.0/16 --pod-network-cidr 10.1.0.0/16
export KUBECONFIG=/etc/kubernetes/admin.conf
kubectl taint node debian node-role.kubernetes.io/control-plane:NoSchedule-
kubectl label node debian node-role.kubernetes.io/worker=

helm install flannel --set podCidr="10.1.0.0/16"  https://github.com/flannel-io/flannel/releases/latest/download/flannel.tgz -n kube-flannel
```

## Post Requirements

- Install metallb
- Install ingress-nginx
- Install cert-manager
- Install StorageClass

## How To

### Reattach disk

```bash
lsblk
mkdir /data
vim /etc/fstab # /dev/sdb1	/data	ext4	defaults	0	2
mount -a
```
