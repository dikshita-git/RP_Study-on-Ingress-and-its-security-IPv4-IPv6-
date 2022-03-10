### Description of the demo:
------------------------------

1. config:
============

This folder contains the traefik.yml. In our case, this file is created inside /etc/traefik folder.


2. docker-compose.yml :
========================

This file contains the version of docker/compose with the declaration of service as traefik and volumes are mounted.

This docker-compose file is deployed at portrainer at TCP port 9000 of the virtual machine by creating a Stack named "Traefiktest" and deploying the test with this docker-compose file. An illustrator is given below:
![portrainer](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/portrainer.PNG)
This image above shows the stack "Traefiktest" created on portrainer and the docker-compose file is put in it and the stack is deployed.

Next step, we click on the "Containers" tab in portrainer and it shows the stack that we created alongwith the ports. This is shown below:
![portrainer_container](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/portrainer_container.PNG)

Now, since our container is created so we can access the Traefik Dashboard by clicking the port:8080:8080. The traefik dashboard looks like the below:

![traefik_dashboard](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/tarefik_dashboard.PNG)

After exploring the traefik dashboard a bit, let us do an example by deploying an nginx web server on portrainer. For this we create a container in the Portrainer dashboard but this  time we have to select the Traefiktest network, which was used to create the stack otherwise the traefik container we want to create now will not connect to the applications it want to expose.Below is the image for the same:
![nginx_container](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/nginx_container.PNG)

Now, we need to configure Traefik in order to expose this nginx container. For this, we could the different wazs under Dynamic Configuration process of Traefik. We can choose to use whether Entrypoints and Routers and create a new Router for every container in the configuration file like it is shown below as example:

------    Reference:  https://doc.traefik.io/traefik/routing/routers/ ------

## Dynamic configuration

    http:
  
        routers:
    
            my-router:
      
                rule: "Path(`/foo`)"
      
                service: service-foo


But for this demo, we are using Dynamic Configuration in <u>Docker Labels</u>. <a href="https://doc.traefik.io/traefik/providers/docker/">Click here</a> to read the refernce 

***This has its benefit in a way because then we do not need to restart Traefik everytime when we want to expose a new container.***

Now, in this case, we next go to the Portrainer dashboard and select the container "nginx" which we created and add some Labels to it. Here:

1. The first label we are adding here in order to enable Traefik monitor this nginx container is: "traefik.enable" and set this to "true" as shown in figure below:
![nginx_container_label](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/nginx_container_label.PNG)

When we set the traefik.enable as "true", only then Traefik will recognize any changes in the label configurations of this container and will react/respond to it.
Before moving further, it is very important to undertsand that Traefik works in a very specific way to configure it:
     
 a) First of all, we need to define an ***Entrypoint***
                 
Entrypoint is where the clients sends a request and the Traefik reverse-proxy will pick it up and once Traefik decides what to do with this Entrypoint, it will route it to an HTTP router and in this HTTP router, we can configure what should actually happen to this request. Also, we can configure if it should pass through some Middlewares incase of any redirections or if we want to make any modifications to the request we can also put some Authentication parts in these Middlewares.Now, this Middleware dorwards the request to the Service and this service is responsible for accessing the Docker container.

The flow of the aforementioned is shown below:
![Traefik_flow](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/Traefik_flow.PNG)
At this point, let us create the Label for the Entrypoint. To do so, we create a label named as "traefik.http.routers.<router_name>.entrypoints"and name it as "web". See reference: <a href="https://doc.traefik.io/traefik/providers/docker"> Click here </a>

Thus our final label looks like below:
![Entrypoint_label](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/update_rule_label.png)


#### Hint: ####
***In our /etc/traefik/traefik.yml, we have declared 2 entrypoints but here we will just consider HTTP. Thus we have named the entrypoint here as "web"***.

After these, we now need to add a Host Name so that Traefik knows which Host Name it should redirect. For this, we configure the HTTP Router with another Label called "traefik.http.routers.<container_name>.rule". In our case the container name is "nginx" so the label is:
![nginx_container_label]

"traefik.http.routers.nginx.rule". In this rule, we can configure which Host Names we want to attach to this container. Let us put Host names as: Host(`ingress.pixxelldesign.com`). This host will point to the public IP address of the server. The rule label is as shown below in the image:
![rule_label](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/new_rule_label.PNG)

Let us deploy the container.  Now, we can reload Traefik dashboard and under "HTTP" we could see the host name that we created as shown in the image:
![host_name](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/my_host_name.PNG)

Below shows a more detailed description(click on the host name) of the route that the TRaefik will take:
![host_name_route](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/host_name_routes.PNG)

So, we see that it:
First comes to the Entrypoint that we configured on Port 80 
        
 ↓
        
It send the request then to nginx routers which will then 
        
↓
        
Forward it to the nginx Server (under Service)

        
        
If we click on "HTTP Services"  tab in Traefik dashboard, we get the below details of all the HTTP services running:
![http_service] (https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/http_service.PNG)

where we select the host name "example.com" and then we have the below details:
![redirection] (https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/redirection.PNG)

Here under "Servers" tab we see an IP address which basically means that it redirects which is our docker container nginx's IP address. We can cross check if our container has the same IP address as shown in the Traefik dashboard by following the command as shown below:
![ip_address] (https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/ip_add_nginx.PNG)

Under tab "Service Details" we see the type as Loadbalancer which means we can add multiple servers we want to loadbalance the TRaefik to different containers that are may be in Docker swarm cluster etc. 

If we open the domain example.com in browser, we see it is not yet secureed because we have not used HTTPS entrypoint yet.

----------------------------------------------------------------

### HTTPS certs ###

Now, let us put the HTTPS entrypoint too in order to give the secured certificate to our domain. IN this case, if we check our traefik.yml file in /etc/traefik folder, we find that under ***Certificate Resolvers*** and we have 2 certificate resolvers namely ***"Staging"*** and ***"Production"***. Here, let us test using the ***" Staging"*** because in Production server of Lets Encrypt , if we have any misconfigurations then we might hit the rate limit. See refernce: <a href="https://doc.traefik.io/traefik/https/acme/">Click here</a>.

One of the advantage of Traefik is that, we do not need to modify the configuration files of Traefik itself even if we want to change the configuration to expose our nginx container, we can directly go to the Portrainer dashboard and configure/change the ***Labels*** of the particular container.Below image shows the new label created for the nginx container.
The label used for exposing the nginx container is: ***traefik.http.routers.<router_name>.tls***. <a href="https://doc.traefik.io/traefik/routing/providers/docker/">Read More</a>
![SSL_label] (https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/SSL_label.PNG)


*** Since, we want an SSL certificate and as per declaration in the traefik.yml file of /etc/traefik folder, so now in the entrypoint label in portrainer dashboard, we have to switch the value from web to websecure otherwise Traefik will not accept the request on port HTTPS (443) ***

Now, we also need to create another label in the container in order to obtain a trusted SSL cert. We will create a new label for this named: ***traefik.http.routers.<router_name>.tls.certresolver*** and we put the name of the certresolver as ***"staging"*** since we want to use staging cert resolver(As mentioned in our traefik.yml) and lets re-deploy the container.

If we check, right now our site looks unsecure like below:
![unsecure_ingress] (https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/unsecure_pixxelldesign.PNG)

Now, if we write https:// in front of ingress.pixxelldesign.com, then we can see it is secured as shown in below image:
![secure_ingress] (https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/secure_ingress.PNG)

--------------------------------------------------------------

### HTTPS REDIRECTION ###

We want the redirection because we do not want to accept the traffic on port 80 anymore and also we want to redirect FROM HTTP to HTTPS in case anyone tries to access the ingress.pixxelldeign.com using port HTTP otherwise the client will see the Error 404 or we dont see the SSL cert in the http://ingress.pixxelldesign.com. To do this redirection, we can configure using Labels too but then it would applz only for that particular container. Since, we want every container in the future to get this redirection, so we shall configure it in the steady configuration file in traefik.yml in /etc/traefik folder. Thus we activate from line 18-22 which now looks like the below:

    # http:
    #   redirections:
    #     entryPoint:
    #       to: websecure
    #       scheme: https

Thus after activating, my traefik.yml looks like the below image:
![active_traefik_redirection] (https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/active_traefik_redirection.PNG)


Hence, by doing so we allow permanent redirection fro every connection that is incoming in the ***Entrypoint: web*** on port 80(HTTP) and redirect it to ***websecure*** which is port 443(HTTPS).Since we have edited the steady traefik file so we shall restart traefik container in Portrainer dashboard. But before restarting, we will add ***web*** value alongwith ***websecure***to the entrypoint label as shown below:
![latest_label] (https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/latest_label.PNG)






Deploy the container
