# Certificate for k3s Ingress with Traefik

In this particular folder, our primary focus is on using default Wildcard certificate for K3s's in-built Ingress Controller ***Traefik***. To deep dive into the steps followed and their declarative aspects, here is a basic draft about <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Traefik">Traefik</a> to read.

Below marks the steps followed for the tassk with their description:

## Steps:


### 1. Set up the k3s Cluster

Here is my k3s cluster set-up <code> <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Cluster-setup">file</a></code> .


### 2. Install cert-manager (using helm)

Once our cluster is up and running, we can now install Cert-manager. Read more about <code> <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/TLS-in-Kubernetes#cert-manager">Cert-manager</a></code>

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Btraefik/helm_install.PNG" height="300">

Now we could check ns to see if cert-manager is running:

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Btraefik/cert-man_ns.PNG">


### 3. Create and apply ClusterIssuer

Here is my <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Certificate_with_k3s%2Btraefik/cert-manager/ClusterIssuer.yaml">ClusterIssuer</a></code> file

Issuers, and ClusterIssuers are Kubernetes resources that represent certificate authorities (CAs) that are able to generate signed certificates by honoring certificate signing requests. All cert-manager certificates require a referenced issuer that is in a ready condition to attempt to honor the request. <a href="https://cert-manager.io/docs/concepts/issuer/">Read More</a>

I am using the ACME issuer type with DNS01 challenges via Cloudflare. This involves me first needing to get an API token from Cloudflare and then providing it to K3s as a Secret resource as <code>api-token</code> of the ClusterIssuer file.

The benefit of using a ClusterIssuer (over a standard Issuer) will make it possible to create the wildcard certificate in the kube-system namespace that K3s uses for Traefik. Also, note that any referenced Secret resources will (by default) need to be in the cert-manager namespace.


### 4. Requesting a wildcard Certificate

This <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Certificate_with_k3s%2Btraefik/cert-manager/Certificate.yaml">Cerficate.yaml</a></code> file requests for a wildcard ecrtificate for my domain dkrp2.xyz.
