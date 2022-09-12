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

We can now check if our cluster has all the required components using 

```
k3s kubectl get nodes
```


### 2. Install Metallb

We can install Metallb using Helm or by the yaml file.I used the yaml file:

Apply the namespace yaml to create the metallb-system namespace

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.10.2/manifests/namespace.yaml
```

Apply the MetalLB manifest controller and speaker from the metallb.yaml file
```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.10.2/manifests/metallb.yaml
```
These would result in craeting the components as shown in image below:

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Bnginx/2.PNG">
<p><i>Fig: Installing Metallb</i></p>


### 3. Create configmap file to insert IP address pool for Metallb

As mentioned above, Metallb refers to a configmap.yaml file to assign our dservices IP addresses from that pool. We can then apply the file as shown below:

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Bnginx/3.PNG">
<p><i>Fig: Create configmap for Metallb</i></p>

Here is my <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Certificate_with_k3s%2Bnginx/ConfigMap.yaml">ConfigMap.yaml</a></code>


### 4. Insttall nginx ingress controller

I used helm fro this purpose:

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Bnginx/4.PNG">
<p><i>Fig: Install nginx</i></p>

>***Note***
>This will also assign deafult Nginx TLS ecrtificate but we will adjust it with our self-signed certificate in teh further steps.

Now, we can check the services and we see the IP address attached to the Loadbalancer as shown in the below image.


### 5. Insttall cert-manager

Since i already have the cert-manager namespace so we can move forward to installing CRD and cert-manager with the command below:

```
k3s kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.crds.yaml
```

```
k3s kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.yaml
```
