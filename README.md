## Topic

Study on the Ingress resource type in Kubernetes for mapping incoming traffic to services and securing it with certificates. Furthermore investigating and comparing the underlying implementations on the basis of k3s.

## Team

***Supervisor:*** Prof. Dr Martin Leischner

***Mentor:*** Richard Clauß

***Candidate:*** Dikshita Kalita



## Table of Contents


<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/README.md#introduction">1. Introduction</a>

<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/README.md#2-motivation">2. Motivation</a>
  
<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/README.md#3-fundamentals">3. Fundamentals</a>

<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/README.md#4-approach-and-goal">4. Approach and Goal</a>

<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/README.md#5-research-questions">5. Research Questions</a>
  
<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/README.md#6-methodology">6. Methodology</a>

<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Chapters/Results">7. Results</a>

<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Chapters/Summary">8. Summary</a> 

<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/README.md#literatures">9. Literatures</a> 


Link to Wiki pages:  <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki">Click Here</a>




## 1. Introduction
The word Ingress describes traffic originating from sources external to the network under investigation. In the Kubernetes context it is the name of one special resource type, dedicated to forward http and https traffic to a service, based on rules specified by the user. Other protocols are not supported.)

In Kubernetes, Ingress can be briefly described as an API object to serve the same purpose by providing some routing rules to manage the access of the external users to the services in the cluster typically via HTTP/HTTPS. Thus, it also becomes equally crucial to secure and filter the Ingress in order to monitor and control the traffic entering the Kubernetes cluster so that only the valid and legitimate traffic enters while the malicious and unauthorised traffic can be discarded or prevented. Though primarily, Ingress only supports one TLS port- 443 and assumes TLS termination, this project aims to study Ingress and its routing rules in a more detailed manner and how we could combine it with TLS, Let’s Encrypt etc. to secure our incoming traffic into the K3s cluster(mainly using an IPV4 address). Since certificate becomes pivotal when considering TLS or security in the cluster, hence studing about various type of certificates in K3s followed by their implementations and study about automatically renew the certificates up and set them running.




## 2. Motivation

In order to proceed further wwith the topic, the project pivots the belowmentioned points of motivaation:

* Determining the basic possibilities of protocols supported in ingress in kubernetes.

* Distinguish the kubernetes object type "ingress" from the other type of services. 

* Analysing points of diferences between different variations of ingress controllers in the market.

* Studying the necessity and detailed implementation of type of certificates to secure the ingress and analysing ways to auto-rotate them before expiration.



## 3. Fundamentals

The initial step started with gathering the basic operational idea of ingress controller and default/wildcard certificates in kubernetes. Following which, some research was also conducted to analyse the procedures to generate the same and understanding their basic differences in applicability. In addition to this, some demonstrations are also conducted which can be found here under <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Demo"><code>Codes</code></a>.




## 4. Approach and Goal

This project aims to secure the Ingress in a K3s cluster using different Ingress Cntrollers namely Traefik, HaProxy, Nginx and to study their differences in use-cases. Further, it also tests various certificates like self-signed, wildcard and default certificates and analyse automatic certificate rotation.

- to gain insights in the described functionalities, first some research questions shall be conducted. the goal is to answer them in the course of the project




## 5. Research Questions

- 5.1 Basic properties of underlying implementations
  
     - a) What are the principal point of differences between Traefik, HaProxy and Nginx?
        
     - b) Which scenarios fit best for the aforementioned alternatives and what are their operational differences?

 - 5.2 Certificate analysis and comparison
        
     - a) Compare the methods of generating the certificaates with different type of ingress controllers

     - b) Determining the ways to distribute the generated certificate and providing them to the ingress controller

     - c) Can wildcard certficiates be beneficial during operations?
  
     - d) How does the complexity of using self-signed certificates compare to the automatic generation of certficiates in ingress controllers?
    
     - e) When can self-signed certificates be used without losing security?</a>
       
     - f) How is the rotation of expiring certificates realized in ingress in kubernetes?
        
  
  - 5.3 Analysing how all the used certificates could be rotated automatically on startup before they expire and possible ways to recover from expired certificates.
  
  
## 6. Methodology

With the goal of exercising the roadmap produced, some methodologies to implement the same are carried out:

<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Chapters/Methodoloy">Methodology</a>



## Literatures:


<details><summary>Ingress</summary><p>
  
  * <code><a href="https://kubernetes.io/docs/concepts/services-networking/ingress/">Ingress</code></a>
  * <code><a href="https://opensource.googleblog.com/2020/09/kubernetes-ingress-goes-ga.html">Ingress routing types</a></code>
  

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


