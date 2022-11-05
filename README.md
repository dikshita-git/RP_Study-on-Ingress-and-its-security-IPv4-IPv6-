## Topic

Study on the Ingress resource type in Kubernetes for mapping incoming traffic to services and securing it with certificates. Furthermore investigating and comparing the underlying implementations on the basis of k3s.

## Team

***Supervisor:*** Prof. Dr Martin Leischner

***Mentor:*** Richard Clauß

***Candidate:*** Dikshita Kalita



## Table of Contents


<a href="https://github.com/dikshita-git/Research-Project/blob/main/README.md#1-introduction">1. Introduction</a>

<a href="https://github.com/dikshita-git/Research-Project/blob/main/README.md#2-approach-and-goal">2. Approach and Goal</a>

<a href="https://github.com/dikshita-git/Research-Project/blob/main/README.md#3-research-questions">3. Research Questions</a>

<a href="https://github.com/dikshita-git/Research-Project/blob/main/README.md#4-fundamentals">4. Fundamentals</a>
  
<a href="https://github.com/dikshita-git/Research-Project/blob/main/README.md#5-methodology">5. Methodology</a>

<a href="https://github.com/dikshita-git/Research-Project/blob/main/README.md#6-results">6. Results</a>

<a href="https://github.com/dikshita-git/Research-Project/blob/main/README.md#7-summary">7. Summary</a> 

<a href="https://github.com/dikshita-git/Research-Project/blob/main/README.md#8-literature">8. Literature</a> 




## 1. Introduction

The word Ingress describes traffic originating from sources external to the network under investigation. In the kubernetes terminology, it is the name of a special resource type or more specifically a kind of an API object which follows some routing rules defined by the user to direct and forward traffic from the external world to the services running in the kubernetes cluster. Ingress has a strong precedence over NodePort or Loadbalancer in a way that it can integrate the routing rules in a single resource instead of exposing multiple services available for a particular application individually. It defines routing rules on layer 7 for the protocols HTTP and HTTPS. Other protocols are not supported. Additional features are loadbalancing, path-based routing, SSL termination, etc. . These features are implemented by dedicated ingress controllers. Since only HTTP and HTTPS rules are supported they usually listen on port numbers 80 and 443 respectively. There are several variations of ingress controllers available which can also be categorized as ***cloud specific ingress controllers*** which is intended to integrate with the specific cloud loadbalancers and open ***source ingress controllers*** which is defined for more generic uses other than clouds. Some of the most popular and widely used implementations of ingress controllers are Nginx, Haproxy and Traefik. 

Additionally, it also becomes crucial to secure the ingress layer so that only the valid and legitimate traffic enters while the malicious and unauthorised traffic can be discarded or prevented. This also appends with the authentication of the application srunning inside the cluster to which the traffic is intended to. With smaller application which do not uphold frequent releases, this common non-functional requirement can be adjusted wwith manageable amount of complexity. However, the scenario alters with applications having very fast releasing velocities. 

This project aims to compare the differences between the most widely used ingress controllers. Moreover, it also anaylses the underlying methodologies for authenticating and authorising the kubernetes applicationns.

In order to proceed further with the topic, the project pivots the belowmentioned points of motivation:

* Analyse points of diferences between three-most variations of open-source ingress controllers.

* Determine possible prospects to limit the access to the kubernetes application and integration with the identity providers ensuring a secured administrtaion of the cluster.


## 2. Approach and Goal

This project aims to secure ingress in a K3s cluster by investigating the varied methods of certificate generation using the three most popular implementations of open-source igress cntrollers namely Traefik, HaProxy, Nginx.
Further, in order to gain more insights in the described functionalities, some research questions shall be conducted which will be answered in the course of the project on the basis of research and demonstrations.




## 3. Research Questions

- 3.1 Basic properties of underlying implementations
  
     - a) What are the principal point of differences between Traefik, HaProxy and Nginx?

- 3.2 Security in kubernetes

     - a) Determining different methodologies to secure kubernetes components
          
     - a) How can certificates be automatically generated and auto-rotated to secure the ingress?
     
     - b) Authentication and authorization of applications running in the clusterto limit the access.
     
  



## 4. Fundamentals

The initial step started with gathering the basic operational idea of ingress controller and certificates in kubernetes. Extending the research to various methodologies available in order to generate the certificates automatically in order to secure our ingress, some trials and experiments are also conducted. Following which, the associated research questions of the project shall be answered in detail. A more detailed draft of the approaches made can be found here: <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Fundamentals/Fundamentals.md"><code>Visit Fundamentals</code></a>.

 
 
## 5. Methodology

With the goal of exercising the roadmap produced, some methodologies to implement the same are carried out: <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Methodology/Methodologies.md">Visit Methodology</a>




## 6. Results

After a thorough investigation of ingress controller and its implementations and usage of certificates to make the traffic secure, there are some observations and results marked. The <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Chapters/Results"><code>Results</code></a> chapter consists of the answers to the research questions mentioned above. Below are the list of links to the respective chapters consisting of results for the same in a detailed manner :

<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/3.1_Basic_properties_of_underlying_implementations.md">* Research Question 3.1: Basic properties of underlying implementations</a>

<a href="https://github.com/dikshita-git/Research-Project/blob/main/K3s/Chapters/Results/3.2_Authentication_and_authorization_of_applications%20_running_in_the_cluster.md">* Research Question 3.2: Automatic certificates and ingress authentication</a>



## 7. Summary

A short and brief summary of the project is accustomed at this page. <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Chapters/Summary"><code>Visit Summary</code></a>



## 8. Literature


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
  
  * <code><a href="https://www.techtarget.com/searchitoperations/tip/Learn-to-use-Kubernetes-CRDs-in-this-tutorial-example">CRDs </a></code>

  * <code><a href="https://www.howtogeek.com/devops/what-are-kubernetes-custom-resource-definitions-crds/">K8s CRDs </a> </code>
  
</p></details> 


<details><summary>Certificates</summary><p>
  
  * <code><a href="https://ikarus.sg/why-traefik-ingress-controller/">Kubernetes Ingress Controllers</a></code>
  
  * <code><a href="https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/">Ingress Controllers</a></code>
  
  * <code><a href="https://traefik.io/glossary/kubernetes-ingress-and-ingress-controller-101/">What is a Kubernetes Ingress Controller?</a></code>
  
</p></details> 


