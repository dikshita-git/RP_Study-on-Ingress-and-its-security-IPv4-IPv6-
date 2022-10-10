## Methodologies

As an approach to the research questions, firstly a k3s cluster with one master node is set up where two workers nodes were assigned to it. A detailed view of the approach is indicated below:


### * Using Nginx ingress controller : <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Demo/Certificate_with_k3s%2Bnginx">Codebase here</a> 

In this experiment, the cluster is set up without deploying the default loadbalancer in k3s called "servicelb"  using the command:

```
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--no-deploy traefik --no-deploy servicelb" sh -s -
```
 which enables us to use our custom loadbalancer. In this case, Metallb was chosen because we had IP address which could be used as loadbalancer by defining them in the IP address pool mentioned in Configmap.yaml of Metallb. Additonally, it was also intended to experiment if adding an external loadbalancer to the nginx ingress controller works well together. Once the nginx ingress controller is installed using Helm in this case, an ingress file to issue a TLS certificate is created with also defining the host name. As a result of testing self-signed certificate, we could see a "invalid" certificate for the domain specified which derives us an idea about the integration of self-signed certificate results when used and if it can be considered beneficial in any way.


### * Using Traefik ingress controller : <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Demo/Certificate_with_k3s%2Btraefik">Codebase here</a>

This case includes using the default ingress controller of k3s "Traefik" in order to issue a wildcard certificate to our host name. For this, once the cluster is set up, Custom resource definitions are installed before installing cert-manager because cert-manager uses the custom resources to define the resources which the users interact with while using cert-manager like Certificates and Issuers. Once these are defined, we safely install cert-manager which is repsonsible for managing our certificates. After checking the kubernetes namespace using the command:

```
k3s kubectl get namespaces
```
We can confirm if our cert-manager is running in order to proceed to the next step of creating a ClusterIssuer file so that we get a CA signed certificate. Furthermore, a wildcard certifcate is requested for our domain by creating an YAML file called certificate which consists of secretName along with the domain name. As soon as the certificate is ready, we can use it with the ingress and the wildcard certificate generated will start working from Lets encrypt. We can check the same using the cocmmnad:

```
kubectl describe certificate -n kube-system
```

This will show the exact status of the certificate if it is created successfully or not and also a detailed view of the certificate generated.



### * Using HaProxy ingress controller : <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Demo/Certificate_with_k3s%2BHaProxy">Codebase here</a>

Here, the haproxy ingress controller is installed using Helm once the cluster is set up and running. As before, CRDs are installed in advance followed by installing the cert-manager and using the default certificate.


The observations derived from the demonstrations condducted gave a precise idea about the working of ingress controller and their integration along with the certificate genrerations and their scenarios whi h further contributed to enable to jot down the results to the research question raised.
