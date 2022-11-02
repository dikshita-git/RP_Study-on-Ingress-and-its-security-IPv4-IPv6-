# Auto-generate and auto-renew certificate for k3s Ingress with Traefik


As automatic deployments of certificates and their auto-renewal plays a pivotal role in easing the development, hence here is a demonstartion of teh same with <a href="https://dkrp2.xyz/">my domain</a> where I have used cert-manager and integrated it with Lets-Encrypt.

Below marks the steps followed for the task with their description:

## Steps:


### 1. Set up the k3s Cluster

1.1) Install K3s server in any node we wish to make it as control plane or a master using the command below:

```
curl -sfL https://get.k3s.io | sh -
```

1.2) Join the agent or worker nodes to this server node

This is achieved by copying the node-token under <code>/var/lib/rancher/k3s/server/node-token</code> and putting it in 

```
curl -sfL https://get.k3s.io | K3S_URL=https://[IP_address_of_master_node]:6443 K3S_TOKEN="PASTE_TOKEN" sh -
```

### 2. Install cert-manager (using helm)

Once our cluster is up and running, we can now install Cert-manager. Read more about <code> <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/TLS-in-Kubernetes#cert-manager">Cert-manager</a></code>

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Btraefik/helm_install.PNG" height="300">
<p><i>Fig: Installing cert-manager using helm</i></p>


Now we could check ns to see if cert-manager is running:

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Btraefik/cert-man_ns.PNG">
<p><i>Fig: Checking namespace named cert-manager</i></p>


>***Note***
>There is a possibility that <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/TLS-in-Kubernetes#cainjector-in-cert-manager">cert-manager cainjector</code></a> shows the status of "Crashloopbackoff".
>  - In such cases, to get more details, check the log file of cert-manager ca-injector by:

```
kubectl logs -n cert-manager <cert-manager-cainjector-name-found-from-get-pods --namespace cert-manager>
```
>   - Next install the higher version of cert-manager yaml by :
     
```
k3s kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.4.0/cert-manager.yaml
```      

Here I tried to apply a higher version of cert-manager. v1.2.0 already moved to use "admissionregistration.k8s.io/v1".
