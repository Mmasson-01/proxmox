# Installing Kubernetes on Proxmox debian

## Requirements


1. In `/etc/crictl.conf`

    ```txt
    runtime-endpoint: unix:///run/containerd/containerd.sock
    image-endpoint: unix:///run/containerd/containerd.sock
    timeout: 2
    debug: false
    pull-image-on-create: false
    ```

2. In `/etc/modules-load.d/k8s.conf`

    ```txt
    overlay
    br_netfilter
    ```

3. In `/etc/sysctl.d/k8s.conf`

    ```txt
    net.bridge.bridge-nf-call-iptables  = 1
    net.bridge.bridge-nf-call-ip6tables = 1
    net.ipv4.ip_forward                 = 1
    ```

4. Configure containerd default config

```bash
containerd config default |  tee /etc/containerd/config.toml
```

5. Restart containerd service

```bash
systemctl restart containerd
```

6. Restart Kubelet

```bash
systemctl restart kubelet
```

7. Init Kubeadm cluster

```bash
kubeadm init --cri-socket /var/run/containerd/containerd.sock --service-cidr 10.0.0.0/16 --pod-network-cidr 10.1.0.0/16
```
