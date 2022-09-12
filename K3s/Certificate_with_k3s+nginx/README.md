In this particular folder, we are trying to create a self-signed TLS certificate for our domain dkrp3.xyz using nginx ingress controller and Metallb as our loadbalancer. As K3s by default comes loaded with traefik ingress controller and since here we want to work with nginx ingress controller, hence while installing k3s on our master node, we have to disable traefik by the command:

```
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--no-deploy traefik" sh -s -
```