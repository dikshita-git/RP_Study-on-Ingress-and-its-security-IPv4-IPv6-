## Topic

Study on Ingress filtering/securing in Kubernetes and evaluation of its best possible implementation(s) across different platforms.

## Team

***Supervsior:*** Prof.Dr Martin Leischner

***Mentor:*** Richard Clauß

***Candidate:*** Dikshita Kalita


## Introduction

Ingress or incoming traffic is one of the crucial object when it comes to accessing our services externally or from outside of the cluster. In Kubernetes, Ingress can be briefly described as an API object to serve the same purpose by providing some routing rules to manage the access of the external users to the services in the cluster typically via HTTP/HTTPS. Thus, it also becomes equally crucial to secure and filter the Ingress in order to monitor and control the traffic entering
the Kubernetes cluster so that only the valid and legitimate traffic enters while the malicious and unauthorised traffic can be discarded or prevented. Though primarily, Ingress only supports one TLS port- 443 and assumes TLS termination, this project aims to study Ingress and its routing rules in a more detailed manner and how we could combine it with TLS, Let’s Encrypt etc. to secure our incoming traffic into the K3s cluster(mainly using an IPV4 address). Since certificate becomes pivotal when considering TLS or security in the cluster, hence studing about various type of certificates in K3s followed by their implementations and study about automatically renew the certificates up and set them running.

## Goal

This project aims to secure the Ingress in a K3s cluster using different Ingress Cntrollers namely Traefik, HaProxy, Nginx and to study their differences in use-cases. Further, it also tests various certificates like self-signed, wildcard and default certificates and analyse automatic certificate rotation.


## Research Questions:

1. What are the best use-cases and scenarios for using Traefik Vs HaProxy Vs Nginx Ingress Controllers and how they differ in their operations?

2. What are the difference situations of applicability of self-signed, wildcard and default certificates?

3. Analysing how all the used certificates could be rotated automatically on startup before they expire and possible ways to recover from expired certificates.
  

## Tasks:

<b>1.</b> Secure K3s ingress with TLS with ingress controllers 

1.1 <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Certificate_with_k3s%2Btraefik">Traefik</a></code>, 
       
1.2 <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Certificate_with_k3s%2BHaProxy">HaProxy</a></code>, 
       
1.3 Nginx and 
       
1.4 Compare the applicability differences of these Ingress Controllers.
       
1.5 Compare the use-cases of the certificates used above.
    
    
<b>2.</b> Test automatic certificate rotation on startup on atleast one of the aforementioned task.

<b>3. </b> Recovering from the expired or old certificates.


## Literatures:

<details><summary>* Cert-manager</summary><p>
  
  * <code><a href="https://cert-manager.io/docs/">Cert manager</a></code>
  
  * <code><a href="https://cert-manager.io/docs/concepts/ca-injector/">Cert manager Cainjector</a></code>
  
  * <code><a href="https://bryanbende.com/development/2021/07/01/k3s-raspberry-pi-cert-manager">Cert-manager in K3s</a></code>
  
</p></details>

*. <code><a href="https://rancher.com/docs/k3s/latest/en/">K3s</a></code>


  
*. <code><a href="https://www.kubermatic.com/blog/keeping-the-state-of-apps-1-introduction-to-volume-and-volumemounts/">Volumes</a></code>
  
*. <code><a href="https://opensource.com/article/20/3/ssl-letsencrypt-k3s">TLS in K3s</a></code>

 

***Wiki:***

For wiki pages, <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki">Click Here</a>


