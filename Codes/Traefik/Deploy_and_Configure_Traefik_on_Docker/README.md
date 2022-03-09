### Description of the demo:
------------------------------

1. docker-compose.yml : This file contains the version of docker/compose with the declaration of service as traefik and volumes are mounted.

This docker-compose file is deployed at portrainer at TCP port 9000 of the virtual machine by creating a Stack anmed "Traefiktest" and deploying the test with this docker-compose file. An illustrator is given below:

# ![alt text](url)
![portrainer](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/portrainer.PNG)

This image above the stack "Traefiktest" created on portrainer and the docker-compose file is put in it and the stack is deployed.

Next step, we click on the "Containers" tab in portrainer and it shows the stack that we created alongwith the ports. This is shown below:

# ![alt text](url)
![portrainer_container](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/portrainer_container.PNG)

Now, since our container is created so we can access the Traefik Dashboard by clicking the port:8080:8080. The traefik dashboard looks like the below:

# ![alt text](url)
![traefik_dashboard](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/tarefik_dashboard.PNG)



2. config: This folder contains the traefik.yml

