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

# K8s

## What is it?


1. K8s is a system for running and co-ordinating the containerized applications across a cluster of machines
2. It is a portable, extensible, open-source platform designed to completely manage the workloads, lifecycle of containerized applications and services using methods that provide portability, scalability and high-availability(HA).
3. In K8s, users can define how our applications ***should run*** and ***the ways they should be able to interact with other applications or outside world***.

---------------------------------------------------------------------------------------------------------------------------
## Usages


1. Scale our services up or down
2. Perform graceful rolling updates
3. Switch the traffic between different versions of our applications to test the features or rollback the problematic deployments. This process is called ***CANARY DEPLOYMENTS***

---------------------------------------------------------------------------------------------------------------------------
## K8s Architecture


K8s is mainly composed of:

| Master Node   | Worker Node(s) |
| ------------- | -------------  |
| It hosts the K8s Control Plane that controls and manages the whole K8s system  | Worker nodes run the actual application we deploy  |

![](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/K8s_components.PNG)
<p>Fig Fk8s_1: Components in Kubernetes</p>
<p>Source courtesy: Kubernetes in Action by Luksa, Marko</p>

The description of Master server components are:
| Component     | Description |
| ------------- | -------------  |
|   etcd        | <ul><li>It is a fundamental component which is a globally available configuration store.</li><li>It is a lightweight, distributed key-value store that can be configured to span across multiple nodes.</li><li>K8s uses ***etcd*** to store configuration data that can be accessed by each of the nodes in the cluster</li><li>It can be used for service delivery and can help components configure or re-configure themselves according to the up-to-date information.</li><li>Also helps maintain cluster state with features like leader election and distributed locking.</li><li>Like most of the other components in the ***Control Plane***, etcd can be configured on a single master server or in production scenarios, distributed among a number of machines.</li>***Only requirement is that---> it should be network accessible to each of the Kubernetes machines***</ul>|
|  API server/ kube-api server  | <ul><li>It is one of the most important master services</li><li>It is the main management point of the entire cluster as it allows a user to configure Kubernetes workloads and organizational units. </li><li>It is responsible for making sure that etcd store and the service details of deployed containers are in agreement.</li><li>It acts as a bridge between various components to maintain cluster health and distributed information and commands.</li><li>The API server implements a RESTful interface, which means that many different tools and libraries can readily communicate with it.</li><li>Client ***kubectl*** is available----> as a default method of interacting with the K8s cluster from a local computer.</li></ul>  |
|    kube-controller manager   | <ul><li>It is a general service that has many responsibilities.</li><li>***Primarily:***<ul><li>It manages different controllers that regulate the state of the cluster, manage the workload lifecycle and perform routine tasks.</li><li>Eg: A Replication Controller ensures that the number if replicas defined for a pod matches the number currently deployed in the cluster.<p>The details of these operations are written to ***etcd***, where the controller manager watches for changes through the API server</p></li></ul></li><li>When a change is seen, the controller reads the new information and implements the procedure that fulfils the desired state. This can involve scaling an application up or down, adjusting endpoints etc.</li></ul>  |
|   kube-scheduler      | <ul><li>It is the process that actually assigns workloads to the specific nodes in the cluster</li><li>This service reads in a workloads' operating requirements, analyzes the current infrastructure environment and places the work on an acceptable node or nodes.</li><li>Scheduler is responsible for tracking available capacity on each host to make sure that workloads are not scheduled in excess of the available resources.</li><li>The scheduler must know the total capacity as well as the resources already allocated to existing workloads on each server.</li></ul> |
|   Cloud-controller-manager   | <ul><li>Cloud controller-managers act as the glue that allows Kubernetes to interact the providers with different capabilities, features and APIs while maintaining relatively generic constructs internally.</li><li>This allows Kubernetes to update its state information according to information gathered from the cloud provider, adjust cloud resources as changes are needed in the system and create and use the additional cloud services to satisfy the work requirements submitted to the cluster.</li></ul>   |
|  Node server components  |  <ul><li>***Nodes***--> are servers that perform work by running containers</li><li>Node servers have a few requirements that are necessary for communicating with the master components, configuring the container networking and running the actual workloads assigned to each other.</li><li><ul>***Container Runtime:***<li>It is the first component that each node must have.</li><li>Typically this requirement is satisfied by installing and running Docker, but alternatives like ***rkt*** and ***runc*** are also available.</li><li>Container runtime is responsible for starting and managing containers, applications encapsulated in a relatively isolated but lightweight operating environment.</li><li>Each unit of work on the cluster is, as its basic level, implemented as one or more containers that must be deployed.</li><li>The container runtime on each node is the component that finally runs the containers defined in workloads submitted to cluster.</li></ul></li></ul>   |
|   kubelet  (It is a service)  | <ul><li>It is the main contact point for each nodes with the cluster group.</li><li>This service is responsible for relaying/broadcasting the information ***to*** and ***from*** the Control plane services, as well as interacting with the "etcd" store to read configuration details or write the new values.</li><li>This service communicates with the master components to authenticate to the cluster and receive commands and ***:bulb:work***.</li><li>***:bulb:Work:*** It is received in the form of a manifest which defines the workload and the operating parameters.</li><li>The kubelet process then assumes responsibility for maintaining the sate of the work on node server.</li><li>It controls the container runtime to launch or destroy the container as needed.</li></ul>  |
| kube-proxy |  <ul><li>This is a small proxy service that is run on each node server to manage the individual host subnetting  and make the services available to other components.</li><li>This process forwards the request to correct containers, can do the primitive load-balancing and is generally responsible for making sure the networking environment is predictable and accessible but isolated where appropriate </li></ul>   |

---------------------------------------------------------------------------------------------------------------------------
## K8s Objects and Workloads


1. Usually containers are the underlying mechanism to deploy the applications.
2. Kubernetes uses additional layers of abstraction over teh container interface to provide scaling, resiliency/elasticity and life-cycle management features.

![](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/K8s_objects.drawio.png)
<p>Fig Fk8s_2: K8s Objects</p>


### 🟣 <u>1. Pods</u> 

|    Description |  Illustration   |
| -------------- | --------------  |
|  <ul><li>It is the most basic unit</li><li>Containers themselves are not assigned to Hosts, but one or more tightly coupled containers are encapsulated in an object called ***Pod***.</li><li>Pod represents----> 1 or more containers that should be controlled as a single application.</li><li>Pods consists of containers------> that operates closely together, share a life-cycle and should always be scheduled on same node.</li><li>Pods consists of containers----> and they are managed entirely as a unit and share their environments, volumes and IP space.</li><li>Usually pods contain of a main container that satisfies the general purpose of workload and optionally some helper containers that facilitate closely related tasks.</li><li>***Eg:*** Pods may have ----><p><b>1 container</b> => running the primary application server</p><p><b>Helper container</b> => pulling down files to the shared file system when changes are detected in an external repository.</p></li><li>Horizontal scaling is usually ***discouraged*** on the pod level as there are other high level objects more suited for the task.</li><li>Within a cluster ----> Pod represents the process that is running.</li></ul>   | <p align="center"><img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/k8s_pods_new.png">Fig Fk8s_3: Pods</p> |



### 🟣 <u>2. Replication Controller (RC)</u> 

|    Description |  Illustration   |
| -------------- | --------------  |
|  In K8s, rather than working with single pods, we have to manage groups of identical, replicated pods.<p>These are created from pod templates and can be horizontally scaled by controllers "Replication Controller" and "Replica Sets"</p> <ul><li>Kubernetes resource or an object that defines a pod template and control parameters to scale identical replicas of pod horizontally by increasing or decreasing the number of running copies.</li><li>It makes sure that a pod or a homogeneous set of pods (replica of pods) is always up and running.</li><li>This is an easy way to distribute load and increase availability natively within Kubernetes.</li><li>RC is responsible for ensuring that the number of pods deployed in the cluster matches the number of pods in its configuration.</li><li>If the number of replicas in a configuration of a controller changes, the controllers either :green_heart: ***starts up*** or :red_circle: ***kills*** the containers to match the desired number.</li><li>If a pod or underlying hosts fails, the controller will start new pods to compensate.</li><li>RC can also perform rolling updates to roll over a set of pods to a new version one by one, minimizing the impact on application availability.</li><li>If a pod disappears for any reason, such as in the event of  a node disappearing from the cluster of because the pod was evicted from the node, the RC notices the missing pod and creates a replacement pod.<p> These happens only when a pod is managed by a RC. as shown in the Fig Fk8s_4.</p></li></ul><p>In the Fig Fk8s_4, <ul>In Node 1,<li><b>POD A ---></b>is ***not*** managed and created by RC.</li><li><b>POD B ---></b>is managed and created by RC.</li><p>Thus, when Node 1 fails => POD A goes :small_red_triangle_down: ***down*** with Node 1 and is not recreated because there is no RC overseeing it. </p><p>=> But RC notices POD B is missing and :green_heart: ***creates*** a new pod instance.</p></ul></p> <p><u>***Benefits of RC:***</u></p><ul><li>It enables us to easily create multiple pods and then makes sure that number of pods always exists and matches.</li><li><b>When a cluster node fails =></b> it creates the replacement replica pods for those pods which were under the control of RC.</li><li>It enables us to ***scale*** the pods ***horizontally.***</li><li>Allows us to update or delete multiple pods with a single command.</li><li>K8s provides a built-in feature called ***Horizontal Pod Autoscaler (HPA)***(<a href="https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/#:~:text=In%20Kubernetes%2C%20a%20HorizontalPodAutoscaler%20automatically,is%20to%20deploy%20more%20Pods.">Read More</a>) which adjusts the number of replica pods of an application (horizontal scaling). Fig Fk8s_5:</li></ul><p><u>***Parts of RC Template:***</u></p>We create an RC by posting a JSON or YAML descriptor to the Kubernetes API server. <p>A Replication Controller has three essential parts (also shown in figure 6):</p><ul><li>A ***label selector***, which determines what pods are in the Replication Controller’s scope.</li><li>A ***pod template***, which is used when creating new pod replica</li><li>A ***replica count***, which specifies the desired number of pods that should be running</li></ul><p>:orange_book: ***NOTE:*** A ReplicationController’s replica count, the label selector, and even the pod template can all be modified at any time, but only changes to the replica count affect existing pods.</p><p>Refer Fig Fk8s_6 and Fk8s_7</p> | <img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/Latest_RC.drawio.png"><p>Fig Fk8s_4: Replication controller</p> <p align="center">Source: <a href="https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/">Click here</a></p>-------------------------------------------- <p align="center"><img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/HPA.drawio.png"><p>Fig Fk8s_5: HPA</p> <p align="center">Source: Kubernetes in Action by Marko Lukša</p></p>-------------------------------------------- <p align="center"><img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/Parts_RC.png"><p>Fig Fk8s_6: Parts of RC</p> <p align="center">Source: Kubernetes in Action by Marko Lukša</p></p>-------------------------------------------- <p align="center"><img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/Demo_RC_template.png"><p>Fig Fk8s_7: RC Template </p><p align="center">Source: Kubernetes in Action by Marko Lukša</p></p> |


### 🟣 <u>3. Replica Sets</u> 

|    Description |  Illustration   |
| -------------- | --------------  |
|    <ul><li>It is the iteration on RC design with greater flexibility in how the controller identifies the pods it is meant to manage.</li><li>These are the next generation RC.</li><li>Supports the ***"set-based label-selector"***. That is why they can match pods based on the presence of a label-key regardless of its value. <p>***Eg:*** </p><p>***1 RC*** :red_circle: ***cannot match*** pods with the label env=prod and those with the env=dev at the same time. It can only match either pods with env=prod label or with env=dev because it uses ***Equity-Based selectors***</p><p align="center">BUT</p> <p>***1 RS*** :green_heart: ***can match*** both sets of prods and treat them as 1 group.</p></li><li>They are most powerful than RC and are meant to be used as backend for deployments.</li></ul> <p><u>***Preferring RS over RC?:***</u></p><ul><li>A ReplicaSet behaves exactly like a ReplicationController, but it has more expressive pod selectors.</li><li>A ReplicaSet behaves exactly like a ReplicationController, but it has more expressive pod selectors.</li><li>The first thing to note is that ReplicaSets aren’t part of the v1 API, so you need to ensure you specify the proper apiVersion when creating the resource.</li><li>We're creating a resource of type ReplicaSet which has much the same contents as the ReplicationController you created earlier. The only difference is in the selector. Instead of listing labels the pods need to have directly under the selector property, you’re specifying them under ***selector.matchLabels***. </li></ul>  | <p align="center"><img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/Demo_RC_template.png"><p>Fig Fk8s_8: RS Template </p><p align="center">Source: Kubernetes in Action by Marko Lukša</p></p>  |


### 🟣 <u>4. Deployments</u> 

|    Description |  Illustration   |
| -------------- | --------------  |
|  <ul><li>Kubernetes objects that are used for managing pods. </li><li>The first thing a Deployment does when it’s created is to create a replicaset.</li><li>The replicaset creates pods according to the number specified in the replica option. </li><li>Deployments can be used to scale your application by increasing the number of running pods, or update the running application. <p>A Kubernetes Deployment can be in either of 3 states during its lifecycle: </p></li><li><p>The ***progressing*** state indicates that the deployment is still working on creating or scaling the pods. </p><p>The ***completed*** state indicates that the deployment has finished its task successfully, while the ***failed*** state indicates that an error occurred with the deployment.</li></ul>             |                  |
  
  
----------------------------------------------------------------------------------------------------------------------------------------------------

## 🟣 Rules of Kubernetes Basic Model


1. Every pod gets its own IP address.
2. NAT is not required to connect containers in a pod or multiple pods in a node.
3. Agents like system daemon, kubelet get all-access passes
4. Shared namespaces(IP, MAC address)



**********************



## 🟣 Kubernetes Networking involves

This content section is a reference from <a href="https://digitalvarys.com/kubernetes-networking-models/">Link 1</a>, <a href="https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-networking-guide-beginners.html">Link 2</a>


### 1. <u>Container-Container Networking: </u>


<p align="center"><img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/C-2-C-networking.drawio.png"></p>

<p align="center">Fig: Container to container networking</p>



1. Container-to-container networking happens through the pod network namespace. This allows two containers in a same pod to communicate each other via ***Localhost*** and ***Port number***.

   Containers in the same pod are in the same network namespace. That is why communication between containers is possible via localhost while port 
   numbers need to be taken care of in case of multiple containers in the same pod.

***a) Network namespace:***

- Collection of network interfaces (connection between two pieces of the equipment on a network)and routing tables (instruction from where to send the network packets)
- Namespaces are helpful as we can have many network namespaces on the same VM without interference
- Each pod get its own namespace.

   
          NOTE 💡 
         
          - There is a secret container called <b>PAUSE CONTAINER</b> that runs on every pod in K8s.

          - The 1st job of this container is to keep namespace open if all the other containers in the pod die.


2. Network namespace allows us to have a separate network interface and routing tables that are isolated from the rest of the system and operate independently.

3."Eht0" is a virtual ethernet device and a tunnel which connects the pod's network with the node on the pod's side.

This same device is renamed as "VethX" (Why the X? There’s a vethX connection for every pod on the node. So they’d be veth1, veth2, veth3, etc.) in the node's side.

4. In other words, each pod's eth0 device is actually connected to a virtual ethernet device in the node.

5. Typically, in network communication, we view the virtual machine to interact directly wit the Ethernet device Eth0

5. Whilst each pod thinks it has a normal ethernet device called "Eth0" to make the network requests through but in real kubernetes is faking it as it i s just a virtual ethernet connection.




### 2. <u>Pod-Pod Networking (on same node): </u>


<p align="center"><img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/Detail_networking.drawio.png"></p>
<p align="center">Fig: Pod-to-Pod networking in single node overview</p>


1. When a pod makes a request to the IP address of another node, it makes that request through its own eth0 interface. This tunnels to the node’s respective virtual vethX interface.

2. This request made gets to the other pod because the node uses a ***network Bridge***

3. ***Network Bridge:***

- It is used to connect two networks together.
- Whenever a request hits the bridge, teh bridge asks all the connected devices i.e. pods if they have the right IP address to handle the original request.
- If one of the pods responds to the bridge then the bridge stores this information and also forwards the data to the original sender so that teh request can be completed.
- In k8s, it is called ***Cbr0***.
- Every pod in the node is a part of this bridge and this bridge connects all the pods in the same node together.


      As shown in figure above , If, one Pod wants to send data to another Pod,

      * First, it will connect using Pod-1’s interface (eth0) to root network namespace’s virtual interface-0 (veth0).

      (From the Root Network Namespace, root interface (eth0) will create a virtual interface (veth0, veth1,.) for every Pods and which is assigned to 
      each Pod network namespace. Then, every Virtual interface will connect through the virtual bridge which will send and receive data using [ARP] 
      (https://en.wikipedia.org/wiki/Address_Resolution_Protocol) protocol.)

       * Then, Root Network Namespace’s virtual interface-0 (veth0) will connect to the virtual bridge (vbr0).

       * Next, data will send from vbr0 to virtual interface-1 (veth1).

       * Finally, data will be sent from veth1 to eth0 of pod-2’s Network Namespace.




### 3. <u>Pod-Pod Networking (on different node): </u>


<p align="center"><img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/P2p%20_different_node.png"></p>
<p align="center">Fig: Pod to Pod networking on different node </p>


1. Firstly, when the network bridge br0 asks the pods if they have the right IP address, then this time "none" of them will say "YES".

2. Then this bridge will fall back to the default gateway. This goes upto the cluster level and looks for the IP address.

3. At the cluster level, there is a table that maps IP address range to various nodes.

4. Pods on these nodes have been assigned IP address from those ranges.

5. ***Eg:***:   

Kubernetes might give pods on Node 1 addresses like 100.96.1.1, 100.96.1.2, etc. 

Kubernetes gives pods on node 2 addresses like 100.96.2.1, 100.96.2.2, and so on.

Then this table will store the fact that IP addresses that look like 100.96.1.xxx should go to node 1, and addresses like 100.96.2.xxx need to go to node 2.

6. After we’ve figured out which node to send the request to, the process proceeds the roughly same as if the pods had been on the same node all along.





### 4. <u>Pod-Service Networking: </u>

1. In Kubernetes, a service lets you map a single IP address to a set of pods. You make requests to one endpoint (domain name/IP address) and the service proxies requests to a pod in that service.

2. This happens via ***kube-proxy*** a small process that Kubernetes runs inside every node.

3. This process maps virtual IP addresses to a group of actual pod IP addresses.

4. Once kube-proxy has mapped the service virtual IP to an actual pod IP, the request proceeds as in the above sections.




### 5. <u>Internet - Cluster Networking: (Project scenario) </u>

External internet network traffic can be divided into 2 parts:

<b>i) Ingress</b>

- Network traffic from Internet-to-Cluster

<b>ii) Egress</b>

- Network traffic from Cluster-to-Internet


<u>***Ingress - Internet to Cluster Networking:***</u>

* This internet traffic routing to the k8s cluster or in short "ingress" has 2 different implementations:

|   Load-balancer           |     Ingress Controller          |
|---------------------------|---------------------------------|
| <ul><li>Ideally, Cloud providers or proxy servers like NGINX provide the loadbalancer (IP address of the loadbalancer)</li><li>Through this IP address, external world will communicate with the Kubernetes cluster</li><li>Inside the kubernetes, configured iptables will take care of routing to the right pod inside K8s. </li></ul> |    <ul><li>Ingress Controller works with "NodePort" service type of K8s.</li><li>When we use Nodeport type servcie, the K8s master will asign a range of network ports and the iptable rules will rule the respective traffic to the respective pod</li><li>Ingress Object is used to expose NodePort to Internet</li></ul> |

<p align="center"><img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/Ingress_network.png"></p>

<p align="center">Fig: Ingress- Internet to cluster networking</p>

In the figure above,

* The data flow from Internet too the k8ss cluster is taking place using the Ingress load balancer.

* ***Step 1:***

When the traffic comes from the Internet to the Ingress Loadbalancer, then it creates an Ingress Object to identify the "IPTABLES".

* ***Step 2:***

The IPtables rules will know which Nodeport has been assigned to which pod

* ***Step 3:***

Through the Nodeport, the traffic will move to the right pod.


This way, Ingress controller will route the traffic from the internet to the kubernetes cluster



--------------------------------------------------------------------------------------------

# K8s in IPV6

#### Headnote:
------------

The ***NOTE*** in this document consists of 2 types:

<b>1. Knowledge Note</b>

It contains knowledge about the topic or idea marked with 💡 .

<b>2. Error Note:</b>

It contains the solution to appeared error while running the commands ❌ .

********************************************************************************************************************************************************

## 🟣 Introduction


Kubernetes using IPv6 infers that we can assign our IPV6 address to the kubernetes pods and other resources at the same time. Though, the implementation of IPv6 makes the scenario a bit more complex than using IPv4 addresses but it also enables us to save many IPv4 address as they are relatively very narrow and we  could assign many unique public IPs which again infers that each pod in kubernetes can have its own internet-facing address which was not possible with IPv4a addresses.

This advantage comes with a big security responsibility as well since we will have an extra risk factor associated with public IP address which are exposed to internet. Thus the implementation of Kubernetes with IPv6 requires a very tight layer of security.

Enabling this security further also implies that we are securing our Kubernetes as well and saving it from the hackers.

IPv6 in Kubernetes also complicates the Load-balancing because the admins will have to ensure that external resources do not bypass the loadbalancers or ingress controllers by sending the requests directly to the IPv6 address of the Kubernetes Pods instead of teh controller that is assigned to manage teh traffic to it.

Here again, Kubernetes Ipv6 implementation makes the Kubernetes IoT friendly.

-----------------------------------------

## 🟣 Prerequisites to enable ipv6 

Sources referred: <a href="https://kubernetes.io/docs/setup/production-environment/container-runtimes/">Container Runtime</a>, <a href="https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/">Install Kubeadm</a>, <a href="https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-init/">kubeadm Init</a>, <a href="https://github.com/sgryphon/kubernetes-ipv6">K8s using IPv6</a>, <a href="https://projectcalico.docs.tigera.io/networking/ipv6">Configure IPv6 only</a>



### <i>1. Docker Container Runtime:</i>
=========================

Kubernetes using IPv6 addrress is enabled only the versions after 1.23, which was assisted by Dockershim but due to the burden of its management for the Kubernetes maintainer, it is disabled in the later versions of kubernetes which actually support IPv6. Instead of Dockershim, we have the Docker Container Runtime which is to be installed and configured before the installation of Kubernetes. The detailed descriptions are penned below:

First, we have to enable IPv6 forwarding and IP tables.

                cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
                net.ipv6.conf.all.forwarding        = 1
                net.bridge.bridge-nf-call-ip6tables = 1
                EOF
               
                sudo sysctl --system    #Apply sysctl params without reboot

               # Prerequisite packages
                sudo apt-get update
                sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common gnupg2

               # Add Docker's official GPG key:
               curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
                    | sudo apt-key --keyring /etc/apt/trusted.gpg.d/docker.gpg add -

               # Add the Docker apt repository:
               sudo add-apt-repository \
                   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
                    $(lsb_release -cs) \
                    stable"

               # Install Docker CE
               sudo apt-get update
               sudo apt-get install -y \
                       containerd.io=1.2.13-2 \
                       docker-ce=5:19.03.11~3-0~ubuntu-$(lsb_release -cs) \
                       docker-ce-cli=5:19.03.11~3-0~ubuntu-$(lsb_release -cs)

***NOTE : Check docker version before proceeding further as sometimes we might need to purge and remove the docker files(if any) and clean up again to start installing container runtime. If docker version returns a success code, the following setting up of Daemon an creating docker.service.d works as well.***

               # Set up the Docker daemon
               cat <<EOF | sudo tee /etc/docker/daemon.json
               {
                    "exec-opts": ["native.cgroupdriver=systemd"],
                    "log-driver": "json-file",
                    "log-opts": {
                    "max-size": "100m"
                },
                    "storage-driver": "overlay2"
                     }
               EOF

               # Create /etc/systemd/system/docker.service.d
               sudo mkdir -p /etc/systemd/system/docker.service.d

               # Restart Docker
              sudo systemctl daemon-reload
              sudo systemctl restart docker

              # Set docker to start on boot
              sudo systemctl enable docker

<b>To purge Docker,</b> follow these commands:

                   1. sudo apt-get purge -y docker-engine docker docker.io docker-ce docker-ce-cli
                   
                   2. sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce  

                   3. sudo rm -rf /var/lib/docker /etc/docker

                   4. sudo rm /etc/apparmor.d/docker

                   5. sudo rm -rf /var/run/docker.sock

<b>To purge Kubernetes,</b> follow these commands:

                   1. kubeadm reset

                   2. sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube* 

                   3. sudo apt-get autoremove

                   4. sudo rm -rf ~/.kube



### <i>2. Installing kubeadm, kubelet and kubectl:</i>
======================================

                    # Prerequisite packages
                    sudo apt-get update
                    sudo apt-get install -y apt-transport-https curl

                    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

                    cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
                    deb https://apt.kubernetes.io/ kubernetes-xenial main
                    EOF

                    sudo apt-get update
                    sudo apt-get install -y kubelet kubeadm kubectl
                    sudo apt-mark hold kubelet kubeadm kubectl



### <i>3. Setting up Kubernetes Control Plane:</i>
======================================

sudo kubeadm init :bulb:

:bulb: <b>Kubeadm </b>

              Kubeadm is a tool built to provide kubeadm init and kubeadm join as best-practice "fast paths" for creating Kubernetes clusters. 
              During kubeadm init, kubeadm uploads the ClusterConfiguration object to our cluster in a ConfigMap called kubeadm-config in the kube-system 
              namespace. This configuration is then read during kubeadm join, kubeadm reset and kubeadm upgrade.


***NOTE : It is possible that kubeadm init returns an error of swap and container runtime not running. In that case disable swap by ***swapoff -a*** and run these commands:***

                    1. sudo rm /etc/containerd/config.toml
                    
                    2. sudo systemctl restart containerd

                    3. sudo kubeadm init

***NOTE:***

Sometimes while running <b>sudo kubeadm init</b>, there might be fatal errors showing ports are in use. It is because <b>sudo kubeadm init</b> command is already run. In that case, <b>sudo kubeadm reset</b> command is to be run which will unmount the ports and then we can run <b>sudo kubeadm init</b> with success.



--------------------------------------------------------------------------------------------------------------------------------------------------

## Fundamentals of Reverse-proxy


**Reverse-proxy:**

If we as a client want to run a web application and host it on some server, and if a Reverse-proxy sits in between the Client and server and takes the connection from the client and forwards it to the server where it is processed. This use of reverse-proxy is beneficial in many ways:

_**<u>1. It helps us in security purposes:</u>**_

Reverse-proxy can handle SSL certificates to decrypt the connection between the client and the server without any additional configuration or certificate management on the server itself.

This is often used to expose any unsecured protocols or to off-load the encryption and decryption process from the server for performance basis.


_**<u>2. It helps us to load-balance the connections:</u>**_

Eg: If our application is running on multiple servers, we can use a Reverse-proxy acting as a Load-balancer to route the connections to multiple target servers. This allows us to **scale the applications dynamically** meaning we can add or remove servers at any time based on the number of requests we get.

It is essentially cloud environments. Eg: If we are using technologies like Kubernetes, to set up a HA cluster, it helps us to scale and deploy the applications.

**Traefik** comes here in the picture as it a flexible tool. Below are some situations where Traefik becomes the game-player:

1. If we are running a Docker host and want to protect all the services with trusted SSL certificates automatically,
2. If we are running a Kubernetes production cluster in the cloud where we need Ingress Controllers, or we might also want to add a digital authentication services to it.....

***
### Introduction to Traefik
---------------------------

1. An open-source and cloud-native Edge Router, HTTP reverse-proxy and load-balancer for HTTP and TCP that is very easily integrated with Docker, Kubernetes, Lets Encrypt etc.
2. It is very easy to integrate with Docker and Lets Encrypt and much more features
3. It has automatic TLS
4. Supports Auto-Service Discovery
5. Supports HTTP2, WebSockets
6. High Availability(HA) mode is available


***
### Working Principle
------------------------
![](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/Traefik_working.PNG)

Traefik has 4 fundamental concepts:


_**1. ENTRYPOINTS:**_

a) Entrypoints are the network entry points into Traefik and are the ports where Traefik listens for traffic. 

b) They can be defined using:

      *A port: 80, 443,….

      *SSL (Certificates, keys, authentication with a client certificate signed by a trusted CA)

      *Redirection to another entrypoint. _**Eg:**_ redirection from HTTP--->HTTPS


_**2. ROUTERS:**_

a) They are the ones who listen to entrypoints and match domains and paths to services.

b) A route has a rule, which defines the route, a service and a set of middleware.


_**3.MIDDLEWARE:**_

a) They run in between Router and Service

b) They can modify the request and response.

c) Traefik has useful middlwares for adding headers, redirecting, rate limiting etc.


_**4. SERVICES:**_

a) They are our applications to where traffic should be routed.

b) A service may be a single container or  a multi-container in a load-balancing set-up.

c) Eg: of services: HTTP, UDP or TCP


***
### Why do we need it?
-----------------------

1. It makes deployment of microservices easier.

2. Traefik integrates with our existing infrastructure components and configures itself automatically and dynamically.

3. Traefik comes with a powerful set of middlewares that enhances its capabilities to include Load-balancing, API Gateway, orchestrator Ingress, east-west service communications and more.


***
### Configuration
-----------------

Two sections/ways to do it:

**1. Dynamic routing Configuration**
**2. Static / startup Configuration** :  This can be done in 3 different ways:
                 <ul>
                  <li>In a configuration file (Check my <a href="">Demo</a>)</li>
                  <li>In command line arguments</li>
                  <li>As the environment variables</li>
                 </ul>


***
### TLS and Traefik
--------------------

1. Traefik supports TLS both with manually defined keys and through Lets Encrypt

2. When using Lets Encrypt: Traefik will automatically renew certificates when needed and automatically provision them when new services are added

3. Traefik supports HTTP and DNS challenges although more configuration might be needed for DNS

4. Traefik stores their certificates in custom JSON format rather than storing it in .crt and .key format


***
### Its Use cases
------------------

### _**1. Load Balancing**_  
 <details>
  <summary>Read More</summary>
  <ul>
<li>Traefik is a modern reverse proxy and load balancing server that supports layer 4 (TCP) and layer 7 (HTTP) load balancing.</li>
<li>It means to distribute incoming HTTP(S) and TCP requests from the Internet to front-end services that can handle these requests.</li>
<li>Its configuration can be defined in JSON, YML, or in TOML format.</li>
<li> It consists of entry point (frontend), service (backend), router (rules), middlewares (optional features).</li>
<li><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/Codes/Hands-on_with_Traefik/Traefik_as_Loadbalancer">Click here</a> for demo</li>
</ul>
  
</details> 

### _**2. API Gateway**_  

### _**3. Kubernetes Ingress**_  

### _**4. Certificate Management**_  

