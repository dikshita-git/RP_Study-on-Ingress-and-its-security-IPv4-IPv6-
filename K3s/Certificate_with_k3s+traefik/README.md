# Certificate for k3s Ingress with Traefik

In this particular folder, our primary focus is on using default Wildcard certificate for K3s's in-built Ingress Controller ***Traefik***. To deep dive into the steps followed and their declarative aspects, here is a basic draft about <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Traefik">Traefik</a> to read.

Below marks the steps followed for the tassk with their description:

## Steps:

***1. Set up the k3s Cluster***

Here is my file k3s cluster set-up andd running it in detail. <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Cluster-setup">Click here</a>


***2. Install cert-manager (using helm)***

Once our cluster is up and running, we can now install Cert-manager. Read more about Cert-manager <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/TLS-in-Kubernetes#cert-manager">here</a>

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Btraefik/helm_install.PNG">




