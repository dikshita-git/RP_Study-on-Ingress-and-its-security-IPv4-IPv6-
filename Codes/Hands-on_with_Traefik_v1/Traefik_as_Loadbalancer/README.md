
![alt text](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/Traefik_as_loadbalancer.PNG)
Source Courtesy: <a href="https://doc.traefik.io/traefik/routing/overview/">Click here</a>


When we start Traefik, we:
1. First define <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Traefik">entrypoints</a> (these are port numbers)
2. Secondly, connected to these entryoints are <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Traefik">routers</a> which analyze to check if any incoming requests match the set of rules.
      - If they match ----> the router might transform this request using some pieces of middlewares before forwarding them to our services 
      
