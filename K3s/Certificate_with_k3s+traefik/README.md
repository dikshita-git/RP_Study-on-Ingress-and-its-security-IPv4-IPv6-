## Wildcard Certificate for k3s Ingress with Traefik

As we know K3s is a very light distribution of Kubernetes allowing us to implement it with the smaller size end-products like Edge, IoT, ARM devices, Continuous Integration, but together of all the security of Ingress in K3s also plays a very pivotal role to securing our applications from exposing to threats. To explain in depth, lets start with Ingress:

As per the <a href="https://kubernetes.io/docs/concepts/services-networking/ingress/">Kubernetes official document</a>,

> Ingress is an API object that manages external access to the services in a cluster, typically HTTP.
