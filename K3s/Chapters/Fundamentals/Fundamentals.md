## Fundamentals carried out for the project

With reading to officially released documentations by kubernetes as starting point to dive into ingress and ingress controllers, how the ingress controller implementations vary from each other, several ideas were also gathered from couple of other documents online. These knowledge stack is stored in the following wiki pages and some references were also followed:

* Ingress in detail: <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Ingress#introduction-to-ingress">Click here</a>

* TLS in kubernetes : <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/TLS-in-Kubernetes">Click here</a>

* Traefik : <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Traefik">Click here</a>

* Comparison of ingress controllers (lesson 1) : <a href="https://kubevious.io/blog/post/comparing-top-ingress-controllers-for-kubernetes">Click here</a>

* Comaprison of ingress controllers (lesson 2) : <a href="https://www.google.com/search?q=comparison+of+ingress+controllers&rlz=1C1GCEA_enDE961DE961&sxsrf=ALiCzsYYWsVmFuRc8zkETVGrsZhACfAdfw:1665426928045&source=lnms&tbm=isch&sa=X&ved=2ahUKEwi33cfjptb6AhXORPEDHVjpAgQQ_AUoAXoECAIQAw&biw=1280&bih=577&dpr=1.5#imgrc=dUIJBw88gvPwiM">Click here</a>


To further comply the theory with practical implementations, and post reading some documentations about certificates and TLS in kubernetes as a whole, demonstrations of genertaing wildcard, self-signed  and default certificates in k3s using three mentioned variations of ingress controllers and also replacing the default loadbalancer in k3s by metallb are conducted. README files explaining the steps and details are also included in the chapter of codes. The list below consists of the links to the code bases saved in the repository:


* Testing wildcard certificate for k3s ingress using default ingress controller "Traefik". Click <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Demo/Certificate_with_k3s%2Btraefik"><code>Codebase</code></a> 

* Testing a self-signed certificate for k3s ingress using nginx ingress controller and replacing the default loadbalancer by Metallb. Click <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Demo/Certificate_with_k3s%2Bnginx"><code>Codebase</code></a> 

* Testing the default certificate for k3s ingress using haProxy ingress controller. Click <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Demo/Certificate_with_k3s%2BHaProxy"><code>Codebase</code></a> 


💡 **NOTE**
> The code base is subjected to changes and updates and is under working.
