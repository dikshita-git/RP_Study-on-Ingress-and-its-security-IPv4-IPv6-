## Methodologies

As an approach to the research questions, firstly a k3s cluster with one master node is set up where two workers nodes were assigned to it. A detailed view of the approach is indicated beloww:


### * Using Nginx ingress controller : <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Demo/Certificate_with_k3s%2Bnginx">Codebase here</a> 

In this experiment, the cluster is set up without deploying the default loadbalancer in k3s called "servicelb"  using the command:

```
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--no-deploy traefik --no-deploy servicelb" sh -s -
```
 which enables us to use our custom loadbalancer. In this case, Metallb was chosen because we had IP address which could be used as loadbalancer by defining them in the IP address pool mentioned in Configmap.yaml of Metallb. Additonally, it was also intended to experiment if adding an external loadbalancer to the nginx ingress controller works well together. Once the nginx ingress controller is installed using Helm in this case, an ingress file to issue a TLS certificate is created with also defining the host name. As a result of testing self-signed certificate, we could see a "invalid" certificate for the domain specified which derives us an idea about the integration of self-signed certificate results when used and if it can be considered beneficial in any way.


### * Using Traefik ingress controller : <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Demo/Certificate_with_k3s%2Btraefik">Codebase here</a>



### * Using HaProxy ingress controller : <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Demo/Certificate_with_k3s%2BHaProxy">Codebase here</a>
