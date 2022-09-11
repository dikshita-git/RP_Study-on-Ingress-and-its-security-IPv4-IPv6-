## Wildcard Certificate for k3s Ingress with Traefik

As we know K3s is a very light distribution of Kubernetes allowing us to implement it with the smaller size end-products like Edge, IoT, ARM devices, Continuous Integration, but together of all the security of Ingress in K3s also plays a very pivotal role to securing our applications from exposing to threats. To explain in depth, lets start with Ingress:

As per the <a href="https://kubernetes.io/docs/concepts/services-networking/ingress/">Kubernetes official document</a>,

> Ingress is an API object that manages external access to the services in a cluster, typically HTTP. Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

> <img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Btraefik/ingress_structure.PNG" width="1000" height="300">
> <p align="center">Fig: Ingress sending all the traffic to one service, <a href="https://kubernetes.io/docs/concepts/services-networking/ingress/">Image Source</a></p>
> An Ingress may be configured to give Services externally-reachable URLs, load balance traffic, terminate SSL / TLS, and offer name-based virtual hosting. An Ingress controller is responsible for fulfilling the Ingress, usually with a load balancer, though it may also configure your edge router or additional frontends to help handle the traffic. We must have an Ingress controller to satisfy an Ingress. Only creating an Ingress resource has no effect. We may need to deploy an Ingress controller such as ingress-nginx. You can choose from a number of Ingress controllers.

In other words, an Ingress Controller can be defined as an in-cluster application which :

1. Observes the Ingress Objects
2. Reads the Ingress Rules defined within
3. Configures itself in order to route the recived traffic or Ingress based on the Ingress rules defined.
This also implies, when we talk about HTTP/HTTPS traffic in Kubernetes, it specifies port 80/443 respectively on the publicj IP address from which our Kuberneets cluster will receive the traffic.

There aare avrious type of Ingress Controllers available. Some of the most popular ones are:

1. The ***Traefik Kubernetes Ingress*** provider is an ingress controller for the Traefik proxy.
2. The ***NGINX Ingress Controller*** for Kubernetes works with the NGINX webserver (as a proxy).
3. The ***HAProxy Ingress Controller*** for Kubernetes is also an ingress controller for HAProxy.
4. ***Istio Ingress*** is an Istio based ingress controller.

In this particular folder, our primary focus is on issuing Wildcard certificates to K3s using ***Traefik***. To deep dive into the steps followed and their declarative aspects, here is a basic draft about Traefik.


