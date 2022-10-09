## Topic

Study on the Ingress resource type in Kubernetes for mapping incoming traffic to services and securing it with certificates. Furthermore investigating and comparing the underlying implementations on the basis of k3s.

## Team

***Supervisor:*** Prof. Dr Martin Leischner

***Mentor:*** Richard Clauß

***Candidate:*** Dikshita Kalita



## Table of Contents

All Wiki pages:  <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki">Click Here</a>

1. <a href="">Introduction</a>
2. <a href="">Fundemantals</a>
    - if appropriate put here what you already did
3. <a href="">Methodology</a>
    in methodology wiki page:
    To answer the conducted research questions we do...:
    1a) 
       - making a table with properties to answer 1a
    1b) 
       - find a list of szenarios based on the table
    2a)
       - list methods of generating the certificates
       - find main properties and compare them
    2c)
       - Getting insights by creating test deployment
       - write down findings
4. <a href="">Results</a>
    4.1 Comparison of ingress controller implementations
   - if appropriate put here what you already did
5. <a href="">Summary</a> 



## Introduction
The word Ingress describes traffic originating from sources external to the network under investigation. In the Kubernetes context it is the name of one special resource type, dedicated to forward http and https traffic to a service, based on rules specified by the user. Other protocols are not supported.)

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
       
1.3 <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Certificate_with_k3s%2Bnginx">Nginx</a></code> and 
       
1.4 Compare the applicability differences of these Ingress Controllers.
       
1.5 Compare the use-cases of the certificates used above.
    
    
<b>2.</b> Test automatic certificate rotation on startup on atleast one of the aforementioned task.

<b>3. </b> Recovering from the expired or old certificates.


## Literatures:


<details><summary>K3s</summary><p>
  
  * <code><a href="https://rancher.com/docs/k3s/latest/en/">K3s</a></code>
  
  * <code><a href="https://www.kubermatic.com/blog/keeping-the-state-of-apps-1-introduction-to-volume-and-volumemounts/">Volumes</a></code>
  
</p></details>

<details><summary>TLS</summary><p>
  
  * <code><a href="https://opensource.com/article/20/3/ssl-letsencrypt-k3s">TLS in K3s</a></code>
  
  * <code><a href="https://www.thebookofjoel.com/k3s-cert-manager-letsencrypt">TLS in K3s with traefik, cert-manager and Lets-Encrypt</a></code>
  
  * <code><a href="https://sysadmins.co.za/https-using-letsencrypt-and-traefik-with-k3s/">HTTPS using Letsencrypt and Traefik with k3s</a></code>
  
  * <code><a href="https://devopscube.com/configure-ingress-tls-kubernetes/">How To Configure Ingress TLS/SSL Certificates in Kubernetes</a></code>
  
  * <code><a href="https://lachlan.io/blog/using-wildcard-certificates-with-traefik-and-k3s">Using Wildcard Certificates with Traefik and K3s</a></code> 
  
</p></details>

<details><summary>Cert-manager</summary><p>
  
  * <code><a href="https://cert-manager.io/docs/">Cert manager</a></code>
  
  * <code><a href="https://cert-manager.io/docs/concepts/ca-injector/">Cert manager Cainjector</a></code>
  
  * <code><a href="https://bryanbende.com/development/2021/07/01/k3s-raspberry-pi-cert-manager">Cert-manager in K3s</a></code>
  
</p></details> 


<details><summary>Certificates</summary><p>
  
  * <code><a href="https://ikarus.sg/why-traefik-ingress-controller/">Kubernetes Ingress Controllers</a></code>
  
  * <code><a href="https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/">Ingress Controllers</a></code>
  
  * <code><a href="https://traefik.io/glossary/kubernetes-ingress-and-ingress-controller-101/">What is a Kubernetes Ingress Controller?</a></code>
  
</p></details> 


 

***Wiki:***

For wiki pages, <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki">Click Here</a>



------------------------------------------------------------

# Richard's suggestion:

## Topic

Study on the Ingress resource type in Kubernetes for mapping incoming traffic to services and securing it with certificates. Furthermore investigating and comparing the underlying implementations on the basis of k3s.

## Team

***Supervisor:*** Prof. Dr Martin Leischner

***Mentor:*** Richard Clauß

***Candidate:*** Dikshita Kalita


## Table of Contents

All Wiki pages:  <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki">Click Here</a>

1. Introduction
    (you are here)
2. Fundemantals
    - if appropriate put here what you already did
3. Methodology
    in methodology wiki page:
    To answer the conducted research questions we do...:
    1a) 
       - making a table with properties to answer 1a
    1b) 
       - find a list of szenarios based on the table
    2a)
       - list methods of generating the certificates
       - find main properties and compare them
    2c)
       - Getting insights by creating test deployment
       - write down findings
4. Results
    4.1 Comparison of ingress controller implementations
   - if appropriate put here what you already did
5. Summary 




## Introduction and Motivation

- what is an ingress -> external traffic to local
- what is kubernetes ingress
The word Ingress describes traffic originating from sources external to the network under investigation. In the Kubernetes context it is the name of one special resource type, dedicated to forward http and https traffic to a service, based on rules specified by the user. Other protocols are not supported.)

### - what are the basic possibilities 
    -> filtering, supported protocols https/https
- distinguish it from usual service types

### - which alternative implementations exist
    -> traefik, nginx, ...

### - Motivation of comparison
    -> what we want is compare the ingress controllers
    
- securing Ingress with certficates is possible
  - we can compare methods of generating them
  - we can have a look at operations e.g. show ways of distributing them and providing them to the ingress and ingress controller
  - can wildcard certficiates be beneficial during operations
  - how does the complexity of using self-signed certificates compare to the automatic generation of certficiates in kubernetes Ingress controllers. When can self-signed certificates be used without losing security?

### - Motivation of certificates
  - what do we want to improve?

Ingress or incoming traffic is one of the crucial object when it comes to accessing our services externally or from outside of the cluster. In Kubernetes, Ingress can be briefly described as an API object to serve the same purpose by providing some routing rules to manage the access of the external users to the services in the cluster typically via HTTP/HTTPS. Thus, it also becomes equally crucial to secure and filter the Ingress in order to monitor and control the traffic entering
the Kubernetes cluster so that only the valid and legitimate traffic enters while the malicious and unauthorised traffic can be discarded or prevented. Though primarily, Ingress only supports one TLS port- 443 and assumes TLS termination, this project aims to study Ingress and its routing rules in a more detailed manner and how we could combine it with TLS, Let’s Encrypt etc. to secure our incoming traffic into the k3s cluster(mainly using an IPV4 address). Since certificate becomes pivotal when considering TLS or security in the cluster, hence studing about various type of certificates in k3s followed by their implementations and study about automatically renew the certificates up and set them running.

## Approach and Goal

- to gain insights in the described functionalities, first some research questions shall be conducted. the goal is to answer them in the course of the project



This project aims to secure the Ingress in a k3s cluster using different Ingress Cntrollers namely Traefik, HaProxy, Nginx and to study their differences in use-cases. Further, it also tests various certificates like self-signed, wildcard and default certificates and analyse automatic certificate rotation.


## Research Questions:

(ensure readability)

- 1. Basic properties of underlying implementations
- a) what are the major differences between haproxy, traefik, nginx
- b) for which scenarios the alternatives are most appropriate

1. What are the best use-cases and scenarios for using Traefik Vs HaProxy Vs Nginx Ingress Controllers and how they differ in their operations?

- 2. certificate stuff
- a) compare methods of generating them
- b) show ways of distributing them and providing them to the ingress and ingress controller
- c) can wildcard certficiates be beneficial during operations
- d) how does the complexity of using self-signed certificates compare to the automatic generation of certficiates in kubernetes Ingress controllers. When can self-signed certificates be used without losing security?
- e) How is the rotation of expiring certificates realized in Ingress and the Ingress controllers?
      3. Analysing how all the used certificates could be rotated automatically on startup before they expire and possible ways to recover from expired certificates.
  



## Tasks: <- remove this

<b>1.</b> Secure k3s ingress with TLS with ingress controllers 

1.1 <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/k3s/Certificate_with_k3s%2Btraefik">Traefik</a></code>, 
       
1.2 <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/treeTable/main/k3s/Certificate_with_k3s%2BHaProxy">HaProxy</a></code>, 
       
1.3 <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/k3s/Certificate_with_k3s%2Bnginx">Nginx</a></code> and 
       
1.4 Compare the applicability differences of these Ingress Controllers.
       Table
1.5 Compare the use-cases of the certificates used above.
    
    
<b>2.</b> Test automatic certificate rotation on startup on atleast one of the aforementioned task.

<b>3. </b> Recovering from the expired or old certificates.



## Literatures:


<details><summary>k3s</summary><p>
  
  * <code><a href="https://rancher.com/docs/k3s/latest/en/">k3s</a></code>
  
  * <code><a href="https://www.kubermatic.com/blog/keeping-the-state-of-apps-1-introduction-to-volume-and-volumemounts/">Volumes</a></code>
  
</p></details>

<details><summary>TLS</summary><p>
  
  * <code><a href="https://opensource.com/article/20/3/ssl-letsencrypt-k3s">TLS in k3s</a></code>
  
  * <code><a href="https://www.thebookofjoel.com/k3s-cert-manager-letsencrypt">TLS in k3s with traefik, cert-manager and Lets-Encrypt</a></code>
  
  * <code><a href="https://sysadmins.co.za/https-using-letsencrypt-and-traefik-with-k3s/">HTTPS using Letsencrypt and Traefik with k3s</a></code>
  
  * <code><a href="https://devopscube.com/configure-ingress-tls-kubernetes/">How To Configure Ingress TLS/SSL Certificates in Kubernetes</a></code>
  
  * <code><a href="https://lachlan.io/blog/using-wildcard-certificates-with-traefik-and-k3s">Using Wildcard Certificates with Traefik and k3s</a></code> 
  
</p></details>

<details><summary>Cert-manager</summary><p>
  
  * <code><a href="https://cert-manager.io/docs/">Cert manager</a></code>
  
  * <code><a href="https://cert-manager.io/docs/concepts/ca-injector/">Cert manager Cainjector</a></code>
  
  * <code><a href="https://bryanbende.com/development/2021/07/01/k3s-raspberry-pi-cert-manager">Cert-manager in k3s</a></code>
  
</p></details> 


<details><summary>Certificates</summary><p>
  
  * <code><a href="https://ikarus.sg/why-traefik-ingress-controller/">Kubernetes Ingress Controllers</a></code>
  
  * <code><a href="https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/">Ingress Controllers</a></code>
  
  * <code><a href="https://traefik.io/glossary/kubernetes-ingress-and-ingress-controller-101/">What is a Kubernetes Ingress Controller?</a></code>
  
</p></details> 
