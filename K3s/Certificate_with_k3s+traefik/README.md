## Wildcard Certificate for k3s Ingress with Traefik

As we know K3s is a very light distribution of Kubernetes allowing us to implement it with the smaller size end-products like Edge, IoT, ARM devices, Continuous Integration, but together of all the security of Ingress in K3s also plays a very pivotal role to securing our applications from exposing to threats. To explain in depth, lets start with Ingress:

As per the <a href="https://kubernetes.io/docs/concepts/services-networking/ingress/">Kubernetes official document</a>,

> Ingress is an API object that manages external access to the services in a cluster, typically HTTP. Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

> <img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Btraefik/ingress_structure.PNG" width="600" height="300">
> <p>Fig: Ingress sending all the traffic to one service, <a href="https://kubernetes.io/docs/concepts/services-networking/ingress/">Image Source</a></p>
> In the above 
