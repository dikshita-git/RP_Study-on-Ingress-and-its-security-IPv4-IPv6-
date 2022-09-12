## K3s with nginx ingress controller, Metallb loadbalancer and using a self-signed certificate

In this particular folder, we are trying to create a self-signed TLS certificate for our domain dkrp3.xyz using nginx ingress controller and Metallb as our loadbalancer. Metallb could be a considerable option for bare-metal Kubernetes and monitors Kubernetes services as type Loadbaalncer and assigns them <code>External IP</code>from the pool of the IP addresses mentioned inits ConfigMap.yaml file. As K3s by default comes loaded with traefik ingress controller and since here we want to work with nginx ingress controller, hence while installing k3s on our master node, we have to disable traefik by the command:

```
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--no-deploy traefik --no-deploy servicelb" sh -s -
```

## Steps:

### 1. Start the K3s cluster and assign worker nodes

Once the command above is implemented in our K3s server, we can copy the node-token from /var/lib/rancher/k3s/server/node-token and use the command below in our worker nodes terminal:

```
curl -sfL https://get.k3s.io | K3S_URL=https://[IP_address_of_master]:6443 K3S_TOKEN="paste_the_copied_node-token" sh -
```

We can now check if our cluster has all the required components:

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Bnginx/00.PNG">
<p><i>Fig: Check the cluster nodes</i></p>


### 2. Install Metallb

We can install Metallb using Helm or by the yaml file.I used the yaml file:

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.10.2/manifests/namespace.yaml

kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.10.2/manifests/metallb.yaml
```
