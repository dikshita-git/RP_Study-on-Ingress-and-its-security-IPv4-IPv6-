
![alt text](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/Traefik_as_loadbalancer.PNG)
Source Courtesy: <a href="https://doc.traefik.io/traefik/routing/overview/">Click here</a>


When we start Traefik, we:
1. First define entrypoints (these are port numbers)
2. Secondly, connected to these entryoints are routers which analyze to check if any incoming requests match the set of rules.
      - ![#f03c15]If they match </span>----> the router might transform this request using some pieces of middlewares before forwarding them to our services 
      
