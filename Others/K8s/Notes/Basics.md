--------
# DOMAINS

1. Domain Name is the address of our website that we type in the URL bar of browser.
2. Earlier when we wanted to access any website, we had to type its IP address which is very difficult to remember. So domain name solved this problem.


-----------------------
## <u>Working of Domains</u>

Refer to the picture below:

![working_domains](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/working_of_domains.PNG)
<p align="center">Fig B1: Working of domain</p>

1. When we enter a domain name in our web browser, it first sends a request to a global network of servers that form the Domain Name System (DNS).
2. These servers then look up for the name servers associated with the domain and forward the request to those name servers.
3. <b><u>Eg:</u></b> if our website is hosted on Bluehost, then its name server information will be like this:

ns1.bluehost.com

ns2.bluehost.com

4. These name servers are computers managed by your hosting company. 
5. Our hosting company will forward your request to the computer where our website is stored.
6. This computer is called a ***web server***. It has special software installed (Apache, Nginx are two popular web server software). The web server now fetches the web page and pieces of information associated with it.
7. Finally, it then sends this data back to the browser.

-----------------
## <u>DNS Records</u>

1. DNS Records is the smallest entity that stores the precise information about which domain name belongs to which target address.
2. Below is a picture of how the entry looks like:

![dns_record](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/dns_record.PNG)
<p align="center">Fig B2: DNS records</p>

***NOTE:*** 📕

***TTL indicates how long (in seconds) a name resolution that has just taken place is likely to remain valid.***


----------------------------------------
## <u>Commonly used types of DNS Records</u>

|  Type   |   Description    |
|---------|------------------|
|  A (IPv4 Host address) |  <ul><li>Most basic and the most commonly used DNS record type</li><li>Used to translate human friendly domain names such as "www.example.com" into IP-addresses such as 23.211.43.53 (machine friendly numbers).</li><li>A-records are the DNS server equivalent of the hosts file - a simple domain name to IP-address mapping.</li><li>This record type is defined in RFC1035</li></ul>  | 
|  AAAA (IPv6 host address)  | <ul><li>AAAA-record is used to specify the IPv6 address for a host (equivalent of the A-record type for IPv4)</li><li>IPv6 is the new replacement for the old IPv4 address system.</li><li>This record type is defined in RFC3596.</li></ul> | 
|  CNAME (Canonical name for an alias)  |  <ul><li>CNAME-records are domain name aliases.</li><li>Computers on the Internet often performs multiple roles such as web-server, ftp-server, chat-server etc.To mask this, CNAME-records can be used to give a single computer multiple names (aliases).</li><li>***Eg:*** the computer "computer1.xyz.com" may be both a web-server and an ftp-server, so two CNAME-records are defined: "www.xyz.com" = "computer1.xyz.com" and "ftp.xyz.com" = "computer1.xyz.com".<p>Sometimes a single server computer hosts many different domain names (take ISPs), and so CNAME-records may be defined such as "www.abc.com" = "www.xyz.com".</p></li><li>The most common use of the CNAME-record type is to provide access to a web-server using both the standard "www.domain.com" and "domain.com" (with and without the www prefix). <p>This is usually done by creating an A-record for the short name (without www), and a CNAME-record for the www name pointing to the short name.</p></li><li>CNAME-records can also be used when a computer or service needs to be renamed, to temporarily allow access through both the old and new name.</li><li>A CNAME-record should always point to an A-record and never to itself or another CNAME-record to avoid circular references.</li></ul>  | 
|  MX (Mail eXchange)     |  <ul><li>MX-records are used to specify the e-mail server(s) responsible for a domain name.</li><li>Each MX-record points to the name of an e-mail server and holds a preference number for that server.</li><li>If a domain name is handled by multiple e-mail servers (for backup/redundancy), a separate MX-record is used for each e-mail server, and the preference numbers then determine in which order (lower numbers first) these servers should be used by other e-mail servers.</li><li>If a domain name is handled by a single e-mail server, only one MX-record is needed and the preference number does not matter.</li><li>When sending an e-mail to "user@example.com", your e-mail server must first look up any MX-records for "example.com" to see which e-mail servers handles incoming e-mail for "example.com".</li></ul>  | 
|  NS (Name Server)     |  <ul><li>NS-records identify the DNS servers responsible (authoritative) for a zone.</li><li>A zone should contain one NS-record for each of its own DNS servers (primary and secondaries).</li><li>This is mostly used for zone transfer purposes (notify messages).</li><li>These NS-records have the same name as the zone in which they are located.</li><li>An NS-record identifies the name of a DNS server - not the IP-address. <p>Because of this, it is important that an A-record and/or AAAA-record for the referenced DNS server exists (not necessarily on your DNS server, but wherever it belongs), otherwise there may not be any way to connect with that DNS server.</p></li></ul>    |  
|  PTR (Pointer)     |  <ul><li>PTR-records are primarily used as "reverse records" - to map IP addresses to domain names (reverse of A-records and AAAA-records).</li></ul>  | 
|  SOA (Start Of Authority)     |  <ul><li>Stores important information about a domain or zone such as the email address of the administrator, when the domain was last updated, and how long the server should wait between refreshes.</li></ul>   |      
|  SRV (location of service)     |  <ul><li>SRV-records are used to specify the location of a service.</li><li>They are used in connection with different directory servers such as LDAP (Lightweight Directory Access Protocol), and Windows Active Directory, and more recently with SIP servers</li></ul> |
|  TXT (Descriptive text)     |  <ul><li>TXT-records are used to hold descriptive text.</li><li>They are often used to hold general information about a domain name such as who is hosting it, contact person, phone numbers, etc.</li><li>One common use of TXT-records is for SPF</li></ul>   | 


------------
# NAMESERVERS

1. They manage the information about which IP addresses belong to which domains.


-------------------------------------------------------------------------------------------------------------


# DOCKER


### Brief History
----------------------------
* Before Containerization: 

![](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/History_of_issues.png)

While _Testing_, the software did not run properly in the Tester's machine as it runs smoothly in the developer's. Possible reasons predicted were:
            * Code Compatibility issues
            * Versions in programming languages, environments issues.
To resolve these issues, the technique of **Containerization** was driven into the picture. Thus, with this method, the Developer instead of sending the raw code base to the tester in order to test the software, now wraps up his code, its dependencies, environments etc. together and deposits in a container and ships/sends that container to the tester and later the tested software moves forward to the production.

In short, **_"Container------->_** is the software that wraps up all the parts of the code along with its dependencies into a single deployable unit that can be used or run in different systems and servers regardless of their different configurations.

This gave birth to **Microservices**---> which simply means: _Multiple isolated containers launched or run together_.

These microservices can be easily managed by using **Orchestration Tools** Eg: _Docker swarm, Kubernetes etc._

***

### Containers vs VM
----------------------

|   Criteria    |   Container   |        VM      |
| ------------- | ------------- |----------------|
|     OS        | <ul><li>When containers put **OS** inside it, it puts only the **_Bare minimum parts_** of the OS required to run the software.</li><li>It avoids the GUI, or other apps (music etc.) in the OS.</li><li>Thus if the original OS is of 2GB then in containers, it might be of 70 MB.</li><li>Thus updates are easy and simple</li></ul> | <ul><li>In VM, we will have the complete OS unlike the containers</li><li>Thus, if the OS is of size 2GB, then in VM too it will be the same size as nothing gets discarded like in containers</li><li>Thus, making it more bulkier than containers</li><li>Updates are time consuming and tough</li></ul>  |
| ARCHITECTURE  | ![](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/container.PNG) <p>Image courtesy: <a href="https://www.weave.works/blog/a-practical-guide-to-choosing-between-docker-containers-and-vms">Click here</a></p><ul><li>Containers behave like it is a software in our host environment/OS</li><li>It shares the same OS to its apps and services</li><li>It takes the resources only from the kernel of the host OS</li><li>There is usually no isolation but containers by default have adequate isolation</li></ul>  |  <ul>![](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/vm.PNG) <p>Image courtesy: <a href="https://www.weave.works/blog/a-practical-guide-to-choosing-between-docker-containers-and-vms">Click here</a></p><li>Here the OS occupies different spaces in the host OS</li><li>There is no interaction between the different OS systems and just takes the resources from the same hardware via Hyper-visor</li><li>There is vast isolation</li></ul>  |
| ISOLATION     | <ul><li>Not so good or complete isolation as of a VM but it is adequate</li></ul>  |   <ul><li>VM provides complete isolation from the concerning host system and is also more secured.</li> <li>So, in case the host OS faces some malware attacks then the VM is not affected. </li></ul>  |
| EFFICIENCY    | <ul><li>More efficient than VM</li>Because they use the energy only for the most necessary parts</li><li>They act like softwares in host OS.</li><li>Uses less resources compared to VM</li></ul>  |  <ul><li>Less efficient as they have to manage the full-blown guest OS</li><li>They have to access resources through hypervisor</li></ul>  |
| PORTABILITY   | <ul><li>As containers are self-contained environments, they can be easily sent  or transferred from one OS to another</li></ul>  |  <ul><li>VMs are not that easily ported with the same settings from one OS to another</li></ul>  |
| SCALABILITY <p>Increased no. of services</p>  | <ul><li>Scaling is very easy here because we can easily add or remove a container on the basis of the requirements due to their light-weight.</li></ul>  | <ul><li>Scaling is tough because they are heavy so it can't be easily removed or added.</li></ul>  |
| DEPLOYMENT    | <ul><li>They can be easily deployed using Docker CLI or making use of the cloud services like Azure, AWS etc.</li></ul>  |  <ul><li> They can also be deployed locally using Powershell by using the VM manager or using the cloud services like Azure, AWS etc.</li></ul>|


***
### Why do we need Containers ?
-----------------------------------------
1. Consistent development environments
It simply implies that in our development process, the code base remains in the same env. where all the processes run as well.
_Eg: Build Process, Testing Process, Staging, Deployment etc._
      
2. To implement microservices
_Eg: Let us assume to have a food delivery app which has 4 features like placing order, delivery tracking, order tracking and payment._

In this case, we can use containers and put all the 4 features in them and connect all of them together. Thus, in case of any interdependency, it can be fulfilled smoothly. 


***
### Introduction to Docker
---------------------------
1. It is an open-source tool that helps us create these aforementioned containers 
2. Which also implies that it helps us indirectly in developing, building, deploying and executing the softwares in _**isolation**_
3. _Eg:_ If we have Docker installed on our PC, then we could put our software in it and deploy it.
4. Docker also provides a very strong layer of security.

***
### Why Docker?
---------------------------
1. Simple to use
2. Fast
3. Easy point of collaboration like in Docker Hub

***
### Docker Architecture
---------------------------
![](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/docker-architecture.png)
Source Courtesy : <a href="https://geekflare.com/docker-architecture/">Click here</a>

**_Description:_ **

_DOCKER CLI :_

**1. docker build:**
<ul>
<li>Builds images</li>
<li>When we pass the _"docker build"_ command to the Docker CLI, it takes the API and gives it to Daemon</li>
<li>The Daemon reads it and then it sees which image (Eg: Ubuntu, Wordpress etc) is to be build</li>
</ul>


**2. docker pull:**
<ul>
<li>This command tells Daemon that it wants an image anyhow from any registry like Docker registry etc.</li>
<li>Daemon reads this request and starts searching for the requested image in Docker registry.</li>
<li>As soon as Daemon finds the requested image, it replicates the image from Docker registry to its own system</li>
</ul>

**2. docker run:**
<ul>
<li>This command runs the images</li>
<li>When this command is passed in our Docker CLI, it goes to Daemon.</li>
<li>Daemon reads the command to which image it should run.

Eg: If we want to run Ubuntu image, then Daemon replicates the Ubuntu image from the Docker registry in its own system and creates a container taking all its details.</li>
</ul>

***
### Docker Environments
---------------------------

**a) Docker-compose**

It is a **_service with Docker_** that enable us to launch multiple containers simultaneously.
```diff
! But how does Docker-Compose run multiple containers simultaneously ?
```
  In order to run multiple containers at the same time, we need to create a YAML file wherein we declare the details like:
  <ul>
  <li>Container Name</li>
  <li>Ports on which these containers will be exposed</li>
  <li>Volumes **declaration and specification which these containers will be using</li>
  <li>Network specification** on which the containers will be in</li>
  </ul>
 
Lastly, after these mentions, we will build (or docker-compose up -d) this file and we can see our containers running.


**b) Docker Swarm**

1. It means to run a swarm / cluster of Docker.
2. We need Docker swarm when we want to run 
```diff
! multiple containers 
in 
@@ multiple nodes @@
```
3. **Eg:** _Let us have 2 servers namely: Server 1 and Server 2_

Both these servers have docker installed and if we want to run the containers on both the servers, then we can use DOCKER SWARM.We just need to include both the servers in DOCKER SWARM, where one of them will be **MANAGER**and the other will be **WORKER NODE**. Post this, we can run the containers inside them.

```diff
- PS: In DOCKER SWARM; we refer : Servers----> nodes and all these nodes will have Docker installed in them individually.
```
4. To manage these multiple nodes, we need orchestration tools like Kubernetes, ECS, Docker swarm etc.


**c) Docker Engine**

<ul>
<li>It is the most important part as it runs the Docker</li>
<li>Any processes or containers that the Docker runs is actually run by Docker Engine</li>
</ul>

_**Parts of Docker Engine:**_

|   DOCKER CLI  |   DOCKER API  |   DOCKER DAEMON|
| ------------- | ------------- |----------------|
|  <ul><li>It is used for interacting with Docker</li><li>It is actually the command line to connect to Docker Daemon with the help of Docker API</li></ul>             | <ul><li>It helps us in interacting with Docker Daemon</li><li>It acts as Messenger as it takes the requests from the CLI and give sit to Daemon.</li></ul> | <ul><li>It is the actual processor </li><li>Docker Daemon is the brain behind all the docker operations</li><li>Thus, making it more bulkier than containers</li><li>It can also be defined as a persistent background process that manages and creates Docker images, container, network, volumes.</li></ul>  |


**d) Docker Objects**

They are:
| Docker Images and  Docker Containers   |   Docker Volumes |  Docker Networks  | 
| -------------------------------------- |------------------|-------------------|
|<ul><li>These are set of instructions that are used to create containers and execute codes inside it.</li><li>Thus, when we say that we have sent a Docker container through a pipeline, we actually refer that we have sent the Docker image through the pipeline. This means the image goes to ----> every system and is created there</li><li>Thus, whenever we have to share any information about the container or even share the container itself, we actually share the images instead of sharing the containers.</li><li>All the instructions needed to run a container are usually stored in Docker images</li></ul>             |<ul><li>It is a permanent storage solution</li><li>Eg: If we have removed a docker container using docker rm -f <container_id>, then usually all the settings like port no., TCP port etc. which we declared in our container will also be removed.Thus, we need a system to store these settings so that in case if the container stops or has been removed, the settings could be stored somewhere. That's where volumes come into picture</li><li>Docker volumes are an efficient way to connect containers to our storage.</li><li>They are more like hard drives which means they are portable from one system to another. This also implies to the fact that they are easily attachable and detachable from any container.</li><li>They store container data.Thus, we can store data of container A using volumes attached and later we can remove the same volume from A and attach it to container B and it will save B's data. But it can be attached one by one and not all together.</li><li>DOCKER VOLUME DRIVERS:<ul><li>It enhances the ability of volumes</li><li>Volumes usually takes some memory from the host OS,and then stores the data. But these volumes are entirely managed by Docker and not by host OS.</li><li>Thus if we want the volume's memory to be stored somewhere else like in cloud, then we will need to use VOLUME DRIVERS</li><li>Docker volume drivers allow us to perform unique abilities like creating persistent storage on other hosts, clouds, encrypt volumes.</li></ul></li></ul> | <ul><li>It is a very powerful object as we can join the containers with Docker networks </li><li>Thus, if we can join the containers, then we can easily communicate within themselves and interact with each other if they have any interdependencies.</li><li>Eg: Let us have -->4 containers but we want to connect-->2 containers.In that case we can put those 2 containers in the docker network and the rest 2 containers will be isolated as the network provides a layer of **Abstraction **or **Isolation **as they are not in the Docker network</li><li>Thus, docker network helps us in managing the containers and isolating them.</li></ul>  


**e) Docker Registry**

It is a dedicated storage location for different types of Docker images along with its different versions.

_**Eg:**_ Docker Hub, Azure container registry, AWS ECR etc.


***
### Dockerfile
---------------------------
1. These are scripts which we can write and then build into an image.
2. The image can then be run in order to create the container anywhere.
3. It is like the shell script.
4. Whatever instructions we want to write for the container, we can write them in our Dockerfile.

Thus, the steps looks like:

User --------------->   Dockerfile  ------------------->  Docker image   --------------------->  Container


***
### Dockerfile Format
---------------------------

|            Format name                     |                                         Description                     |
| ------------------------------------------ | ----------------------------------------------------------------------- |
| FROM<p>Syntax: FROM <base_image></p>      | <ul><li>An instruction taht informs Docker about the base image that is to be used for the container</li><li>**Always used as the first instruction for any Docker image**, but **we can use it multiple times**</li></ul> |
| ADD<p>Syntax: ADD <source><destination></p>  | <ul><li>Used to add new sources from our local directory or an URL to the file system of the image that will become a container in the designated location</li><li>We can include multiple items as source and can even make of wildcards(*-->means all files with the same name ) and if our declared destination does not exist then it will create one. </li></ul> |
| COPY<p>Syntax: COPY <source><destination></p>  | <ul><li>Used to copy new resources from our local directory to the file system of the image that will become container in the designated location</li><li>Similar to ADD; but with a difference that ADD can add a new URL to the file system whereas COPY cannot</li><li>We can include multiple items as source and can even make use of wildcards. IN case, if the destination does not exist then it will create one.</li></ul>|
| RUN<p>Syntax: RUN <command></p>  | <ul><li>It is used to run a specific command that we want to run in the container during its creation</li><li>Eg: if we wan to update Ubuntu instance, then  we can write it as: RUN apt-get update</li></ul> |
| WORKDIR <p>Syntax: WORKDIR <directory></p>    | <ul><li>It is responsible for setting the working directory so that we can run shell commands in that specific directory during teh build time of image. </li><li>Eg: If we want our working directory to be /database then, WORKDIR /database</li></ul> |
| CMD  <p>Syntax: CMD <command></p>      | <ul><li>It tells the container which command to run when it gets started</li><li>RUN vs CMD <ul><li>When the container is created, we use **RUN** command</li><li>When container has started, we use **CMD** command</li></ul></li></ul> |
| ENTRYPOINT     | <ul><li>It sets a default application to be used every time a container is created with the image.</li><li>It defines the commands Docker executes upon container creation.</li><li>Options are overridable in the command line during container startup.</li></ul> |
| EXPOSE     | <ul><li>This instruction tells Docker which port to listen on during runtime.</li><li>It associates a specific port to enable networking between the container and the outside world.</li></ul> |
| LABEL      | <ul><li>It allows you to add a label to your docker image.</li><li>The label instruction defines metadata information for the image.</li><li>We can add as many labels as you see fit in the form of key-value pairs.</li><li>Eg: image metadata could include the version number, author information, description, etc.</li></ul> |
| ENV        | <ul><li>It sets environment variables.</li></ul> |
| VOLUME     | <ul><li>The volume instructions allow you to create mount points from host machine directories or other containers.</li><li>is used to enable access from the container to a directory on the host machine.</li></ul> |
| USER       | <ul><li>This instruction sets the username or UID of the user when running the image or instructions in a Dockerfile such as CMD, RUN, and ENTRYPOINT.</li></ul> |
| MAINTAINER | <ul><li>It defines a full name and email address of the image creator.</li></ul> |


***
### Docker Stack
---------------------------
Before diving into Docker stack, let us get a flash about docker-compose. So, docker-compose is used for creating a multi-container architecture.It can be used to link multiple containers to create a development, testing environments etc.

If we want to deploy this docker-compose within the docker Swarm, then it is called a **Docker Stack**.
As it is lucid that in Docker swarm, our docker containers will be running on a distributed network on multiple servers. Also, in the docker swarm when the Manager and Worker nodes are combined together it is called a Docker Swarm Cluster.

Within this Swarm cluster, if we put docker-compose and we create that multi-container architecture, so that the complete environment like development, testing etc. can be scaled and it can be orchestrated together, this concept is called **Docker Stack**.



**EXAMPLE**

Let us create a Development environment in our Docker swarm cluster. Here, in this environment: 

![](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/docker-stack.jpg)

These 3 replicas should-------> internally communicate with the MySQL Database so that the wp (wordpress) website can be developed.But, these 4 containers namely WP Replica 1, WP Replica 2, WP Replica 3 and MySQL can be running anywhere on the manager or workers because they are in our swarm cluster. For this scenario, our docker compose will look like:

  ---------------------------------------------------------------------------------------------------------------------------------------------------
  
  
