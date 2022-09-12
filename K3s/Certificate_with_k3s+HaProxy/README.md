Here, we are setting up a TLS using HaProxy Ingress Controller meaning all the exteranl traffic passes through this ingress controller which also implies that at thsi very exact point, the TLS can be set up to secure th etraffic.
By doing so, all the services running inside our k3s cluster inherit the TLS. Additionally, hereby the focus is also on setting up certificates using Lets Encrypt.

I used Helm in this context and installed it following <a href="https://helm.sh/docs/intro/install/#from-script">Here</a>.

Below are teh steps followed and a descriptive detail herewith is also available:

## STEPS:

### 1. Add the HaProxy helm repository and update it to have all the latest charts

Commands:

```
helm repo add haproxytech https://haproxytech.github.io/helm-charts
```

```
helm repo update
```

### 2. Install haproxy ingress controller

Command:

```
helm install haproxy haproxytech/kubernetes-ingress --set controller.service.type=Loadbalancer
```
Once done, we will get a response like shown below:

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Bhaproxy/1.PNG">

<p align="center">Fig: HaProxy installation response</p>

Here, ```controller.service.type=Loadbalancer``` will create aa loadbalancer in front of my cluster which will further be used to expose my cluster to Internet so it could be reached from a public DNS.

Now, we could check by:

```
k3s kubectl get pods 
```

### 3. Set up CRD and Cert-amanager using Helm

Command:

```
helm repo add jetstack https://charts.jetstack.io 
```

```
helm repo update
```

```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.crds.yaml
```

cert-manager uses Kubernetes Custom Resources to define the resources which users interact with when using cert-manager, such as Certificates and Issuers. <a href="https://cert-manager.io/docs/contributing/crds/">Read more</a>

```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.yaml
```
This  command will install all the cert manager components in our cluster. Next we need is a ***ClusterIssuer*.



### 4. Create a ClusterIssuer:

Issuers, and ClusterIssuers are Kubernetes resources that represent certificate authorities (CAs) that are able to generate signed certificates by honoring certificate signing requests. All cert-manager certificates require a referenced issuer that is in a ready condition to attempt to honor the request. <a href="https://cert-manager.io/docs/concepts/issuer/">Read More</a>

Here is my <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Certificate_with_k3s%2BHaProxy/Issuer.yaml">ClusterIssuer</a> file.

***Description of ClusterIssuer***

* Its named as Letsencrypt-staging
* Server here specified is the letsencrypt staging server.
* All ACME Issuers follow a similar configuration structure - a clients email, a server URL, a privateKeySecretRef, and one or more solvers. Solvers come in the form of dns01 and http01 stanzas. 

> **Note**

> Automatic Certificate Management Environment (ACME) protocol is a communications protocol for automating interactions between certificate authorities and their users' servers, allowing the automated deployment of public key infrastructure at very low cost. <a href="https://en.wikipedia.org/wiki/Automatic_Certificate_Management_Environment">Read More</a>
