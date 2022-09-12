## Goal

This project aims to secure the Ingress in a K3s cluster using different Ingress Cntrollers namely Traefik, HaProxy, Nginx and to study their differences in use-cases. Further, it also tests various certificates like self-signed, wildcard and default certificates and analyse automatic certificate rotation.


## Research Questions:

1. What are the best use-cases for Traefik Vs HaProxy Vs Nginx Ingress Controllers and how they differ in their operations?

2. What are the difference situations of applicability of self-signed, wildcard and default certificates?

3. Analysing best practices to implement certificate rotation in k3s.
    
        - Automatic rotation



## Tasks:

1.Secure K3s ingress with TLS with ingress controllers 

       1.1 Traefik, 
       
       1.2 HaProxy, 
       
       1.3 Nginx and 
       
       1.4 Compare the applicability differences of these Ingress Controllers.
       
       1.5 Compare the use-cases of the ecrtificates used above.
    
    
2. Test automatic certificate rotation on atleast one of the aforementioned task.



### Roadmap:

***1. Codes:***

* <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Cluster-setup">K3s cluster set-up</a>
* <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Certificate_with_k3s%2BHaProxy">K3s with Haproxy</a>
* <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Certificate_with_k3s%2Btraefik">K3s with Traefik</a>


***2. Wiki:***

For wiki pages, <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki">Click Here</a>
