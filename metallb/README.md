# Configure MetalLB

1. Create a namespace

```bash
k create ns metallb-system
```

2. Move to the namespace

```bash
kn metallb
```

3. Install the metallb manifest

```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.10/config/manifests/metallb-native.yaml
```

4. Apply the configurations

```bash
k apply -f metallb.yml
k apply -f l2.yml
```

5. Ensure the nginx-ingress service is deployed with type LoadBalancer and not ClusterIP
