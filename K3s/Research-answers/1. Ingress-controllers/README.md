## <ins>Question:</ins> 

<i>What are the best use-cases and scenarios for using Traefik Vs HaProxy Vs Nginx Ingress Controllers and how they differ in their operations?</i>


### Introduction

In order to understand Ingress Controller and the operational differences between its types, we have to first get along with Ingress and Ingress Controllers: why do we need them and how do they align to support each other in the kubernetes cluster.

***1. Why do we need Ingress and Ingress Controllers ?***

Let us consider an application named <b>"App"</b> deployed on Kubernetes cluster with 2 number of replicas which run on 2 different Worker nodes inside the cluster as shown below:

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Research_Question/1.%20Ingress/1.drawio.png" width=500>

Now, in order to make our App accessible publicly, we have to create a Service of type Nodeport. Whenever we try to create a service for our application and if the IP address of atleast one of the  worker nodes and port number which is defined as part of the service is known, we can easily access our application from outside. This kind of service is referred to as 🔎 <b>External service<sup>1</sup></b> i.e. we can access our application outside the kubernetes cluster. Once this service receives the request from multiple clients, it will do the 🔎 <b>loadbalancing<sup>2</sup></b> and forwards the request to the application pods running on the 2 worker nodes.

<b>Summary:</b>

       Whenever we define our service with ***-type=NodePort*** , that particular service will be created as ***External Service***.

🟢 It is ***good*** for Development and Testing. 🔴 But not for Production application because there might be high chances that on eof our nodes might be crashed or the complete Kubernetes cluster might crash. In that case, the IP address of nodes and port number might change and additionally, we also do not want to share teh IP address  of worker nodes and port numbers to the clients. Thus, here again we have to create an 🔎 <b>Internal service</b>.

In order to create Internal service, we need to :

1. Define the service type as ***ClusterIP***.
2. This is available only inside the kubernetes cluster and the applications inside that kubernetes cluster can only access our service.
3. This means, it is 🔴not ***public***.
4. Also, it is to be kept in mind that providing the access to the service directly to the client is not recommended due to security reasons.


💡><b>NOTE:</b>
>In order to make our services secure, we can create service as an ***Internal service*** by defining type as ***ClusterIP*** 



