This file consists of a detailed description about Ingress Controllers and the results to my finding for my research question 1.

# PRECAP:

1. <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Research-answers/1.%20Ingress-controllers/README.md#introduction">Introduction</a>
  
  * <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Research-answers/1.%20Ingress-controllers/README.md#-1-why-do-we-need-ingress-and-ingress-controllers-">Why do we need Ingress and Ingress Controllers ?</a>
  * <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Research-answers/1.%20Ingress-controllers/README.md#-2-how-would-we-provide-our-clients-the-access-to-our-application-in-a-secured-way">How would we provide our clients the access to our application in a secured way?</a>

10. <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Research-answers/1.%20Ingress-controllers/README.md#-references">References</a>



## <ins>Question:</ins> 

<i>What are the best use-cases and scenarios for using Traefik Vs HaProxy Vs Nginx Ingress Controllers and how they differ in their operations?</i>


### Introduction

In order to understand Ingress Controller and the operational differences between its types, we have to first get along with Ingress and Ingress Controllers: why do we need them and how do they align to support each other in the kubernetes cluster.



#### ❓ 1. <ins>Why do we need Ingress and Ingress Controllers ?</ins>


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


💡<b>NOTE:</b> 🔦
>In order to make our services secure, we can create service as an ***Internal service*** by defining type as ***ClusterIP***  so that teh service is not accessible outside the cluster.
>Once we create service as ***Internaal service***, then our clients cannot access our service using IP address and port number.



#### ❓ 2. <ins>How would we provide our clients the access to our application in a secured way?</ins>

This is where for ***Ingress*** comes into the picture.

In Kubernetes terminology, we call it as an ***Ingress Resource***. We can create an INgress Resource by defining an YAML file siilar to defining a service. Here is an overview of content of Ingress YAML file: <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Certificate_with_k3s%2Btraefik/ingress.yaml"><code>Ingress</code></a>.
We define:

* ***"kind"*** as Ingress and 
*Iimportant part is under ***"specs"*** or ***specifications*** where we define the set of routing rules with 
 * ***host*** as - Domain name. This using this domain name, the clients can access the application. Domain name is assigned to the ***hsot*** attribute and 
 
Ingress resource alone cannot do all these job and needs an ***Ingress Controller*** which is basically an incrementation of teh Ingress resource. Ingress controller can be simply defined as a pod which is running on one of the worker nodes in our kubernetes cluster. We caan define the iNgress Controller as a ***Daemon set*** or ***Deployment***. If the ingress controller is defined as:

| Daemon Set    | Deployment    |
| ------------- | ------------- |
|<ul><li>A DaemonSet ensures that all (or some) Nodes run a copy of a Pod.</li> <li>A DaemonSet schedules exactly one type of Pod per cluster node, masters included, thus when the ingress controller is defined using DaemonSet, the controllers are created on each worker node.</li> <p>Eg: If we have 6 worker nodes fro our application, then Ingress controller will be created on 6 worker nodes.</p></ul>   | <ul><li>In a reasonably large cluster deploying ingress as deployment with multiple replicas is suitable compared to daemonset.</li><li></li>Sometimes if the ingress-controller is running via a deployment and one of the nodes didn't have the pod allocated then the node could be listed as "unhealthy" in the loadbalancer targergroup healthcheck. </ul>  |





#### ❓ 3. Types of 3rd party Ingress Controllers:

There are lot of Ingress Controllers





#### 📚 References:

* <a href="https://kubernetes.github.io/ingress-nginx/deploy/baremetal/">Kubernetes ingress official</a>

* <a href="https://stackoverflow.com/questions/61004408/is-it-necessary-to-deploy-the-ingress-controller-using-daemonset">Stack Overflow </a>
