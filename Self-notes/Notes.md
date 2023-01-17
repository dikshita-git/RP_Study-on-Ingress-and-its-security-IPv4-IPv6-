## Table of Contents:

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#ingress">1. Fundamentals</a>

  * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#11-introduction">1.1 Introduction</a>

  * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#12-service">1.2 Service</a>
    
  * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#13-ingress-components">1.3 Components</a>

  * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#14-working-principle">1.4 Working Principle</a>

  * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#15-why-is-it-needed">1.5 Why is it needed?</a>

  * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#16-routing-in-ingress">1.6 Routing</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#ingress-security">2. Ingress Security</a>

  * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#21-introduction">2.1 Introduction</a>

  * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#22-tls-and-its-working-principle">2.2 TLS and its working principle</a>

  * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#23-automatic-certificate-generation">2.3 Automatic certificates</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#3-authentication-and-authorization-of-kubernetes-applications">3. Authentication and authorization of kubernetes applications</a>

-----------------------------------------------



# Ingress:


### 1.1 Introduction


1. In K8s, the pods where our applications live are so temporary that they get a new IP address every time they are launched. This is because the pods are destroyed dynamically and recreated with each deployment.
2. Thus, with this process we will lose the track of the dynamically changing IP address of the pods all the time.
3. Hence, we need some technique in which we could refer to it instead of depending on to track their IP address which is changing all the time. Hence, ***Service*** came into play.


### 1.2 Service

Service is a layer of abstraction or isolation that maps to one or more pods. This abstraction will allow other applications or pods to reach the service by simply referring to the "Service name".
<p>Which also means that there is no more dependency on knowing the IP address assigned to the pods anymore. External application and end-users can also access the services given the fact they are exposed publicly to the Internet. </p> 

| Cluster IP   |     NodePort     |    LoadBalancer    |    ExternalName     |     DNS     |
| -----------  | ---------------- |------------------- |-------------------- |------------ |
|  <ul><li>Accessible <u>within the cluster</u>.</li><li>Here, dependent applications can interact with other applications internally using the Cluster IP service.</li><li><b>YAML file of Cluster IP: </b>![cluster_ip](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Others/cluster_ip.PNG)<p>Source courtesy: <a href="https://www.densify.com/kubernetes-autoscaling/kubernetes-service-load-balancer">Click here</a></p></li></ul>   |   <ul><li>NodePort services are accessible <u>outside the cluster</u>.</li><li>It does so by creating a  mapping of ***pods*** with the ***hosting node/machine*** on a static port.</li><li><b>Eg:</b>If we have a node with IP address ---> 10.0.0.20 and a Redis pod running under it. <p>Then NodePort will expose <b>10.0.0.20:30038 (assuming the port number to be 30038)</b>, which we can access outside the K8s cluster.</p></li><li><b>YAML file of NodePort:</b>![nodeport](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Others/nodeport.PNG) <p>Source courtesy: <a href="https://www.densify.com/kubernetes-autoscaling/kubernetes-service-load-balancer">Click here</a></p></li></ul>  |  <ul><li>This service type creates Loadbalancers in various Cloud providers like GCP, Azure, AWS etc. to expose our application to the Internet.</li><li>The Cloud provider then provides a mechanism for routing the traffic to the services.</li><li>This service type is most commonly used in web application or website.</li><li><b>YAML file of Loadbalancer service type</b>![loadbalancer](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Others/loadbalancer.PNG)<p>Source courtesy: <a href="https://www.densify.com/kubernetes-autoscaling/kubernetes-service-load-balancer">Click here</a></p></li></ul>  | <ul><li>For any pod to access an application outside the K8s cluster like the external DB server, we have to use the ***ExternalName*** service type.</li><li>Here, instead of endpoint object (like we saw in rest of the service type under section <b>"spec"</b>), the service will simply redirect to a CNAME of the external server.</li><li><b>YAML file of Externalname service type</b>![external_name](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Others/external_name.PNG)<p>Source courtesy: <a href="https://www.densify.com/kubernetes-autoscaling/kubernetes-service-load-balancer">Click here</a></p></li></ul>  |


1. In Kubernetes, Ingress is the object that allows access to the Kubernetes services from outside of the Kubernetes cluster.
2. Ingress is basically used when we have multiple services in our K8s cluster and we want the user requests to be routed to the service based on their path.
3. In other words, K8s Ingress is a collection of routing rules that govern how external users access the services running in a K8s cluster.
4. Kubernetes Ingress is an API object that provides routing rules to manage the user's access to the services in a Kubernetes cluster typically via HTTP/HTTPS through a single externally reachable IP address.
5. With Ingress, we can easily set up the rules for routing the traffic without creating a bunch of Loadbalancers or exposing each service on the node. This is the best option in production environments.
6. Ingresses operate at the application layer of the network stack (HTTP) and can provide features such as cookie-based session affinity and the like, which services can’t.
------------------------------------
<b>EXTERNAL SERVICE vs INGRESS</b>

| EXTERNAL SERVICE    |       INGRESS      |
| ------------------- | ------------------ |
|                     |                    |
|  <p align="center"><img src="https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Others/Ingress_external_service.png" width=300px;><p><b>Source:</b> <a href="https://kubernetes.io/docs/concepts/services-networking/service/">Click here</a></p></p><ul><li>This is a service of type "Loadbalancer" which means we are opening it to public by assigning an external IP address to the service </li></ul>              |  |



### 1.3 Ingress Components

![Ingress_components](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Others/Ingress_new_components.png)
<p align="center"><b>Fig:</b> Components of Ingress</p>


| Ingress API Object  | Ingress Controller |
| ------------------- | ------------------ |
|  <ul><li>The API object indicates the services that need to be exposed outside the cluster. It consists of the routing rules.</li></ul>                  |  <ul><li>Ingress Controllers are ----> pods just like any other applications. so they are part of cluster and can see other pods too.</li><li> Ingress resources require an Ingress Controller component to run as a Layer 7 proxy service inside the cluster.</li><li>To make the Ingress resource work, an Ingress Controller is needed to be running in the Kubernetes cluster.</li></ul>                  |



### 1.4 Working Principle

![Ingress_working_principle](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Others/New_Ingress_working_principle.png)
<p align="center"><b>Fig:</b> Working Principle of Ingress with ClusterIP service</p>

<ul>    
<li> Ingress can be described like a pod that we deploy in our cluster.</li>
<li> Ingress Controller is a software that has to be installed.</li>
<li> Once we have deployed the pod for the Ingress Controller, the Kubernetes rule says that the pod cannot be directly exposed. It has to have a 
service. </li>
Thus, we have an ***"Ingress service"*** created which is basically a ***NodePort*** so that the services can be accessed by the outside world.
<li>Earlier, we directly accessed the services. But when we are working with Ingress, the rule says that ***A service should be a ClusterIP so that 
the services cannot be accessed directly but only be accessed via the Ingress.</li>
Hence, in the Fig Ingress_2 we define the services as ClusterIP(means they cannot be accessed from outside of the Internet) but since the Ingress 
Controller can access these services and Ingress Controller itself is exposed, thus we get to see or access the services.
<li>Once the Ingress Controller is created, we have to define the rules separately as they are not defined inside the Ingress Controller .</li>
<li> Hence in the Fig Ingress_2, </li>
             
    1. The User sends or visits an URL in the browser, that gets redirected to the Ingress Service
    2. Ingress Service then forwards the request to Ingress Controller
    3. Ingress Controller then checks the Ingress rules and based on these rules, it takes a decision to where the request has to be sent to.
      
<li>Being inside the cluster themselves, Ingress Controllers like other Kubernetes pods have to be exposed to the outside via a service with a type of either **NodePort** or **Loadbalancer**.</li>

![Ingress_intro](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Others/Ingress_intro.png)
<p align="center"><b>Fig:</b> Exposing multiple services via Ingress, Source: Kubernetes in Action by Marko Lukša</p>

<li>In the Fig Ingress_2, <p>There is only one Entrypoint in Ingress(2nd block) that all traffic goes through.</p><p>Each service is connected to 1 Ingress Controller, which in turn is connected to many internal pods.</p><p>The controller has the ability to inspect HTTP requests and thus directs a client to the correct pod based on URL path or domain name it finds.</p></li>
</ul>


### 1.5 Why is it needed?

In order to understand Ingress Controller and the operational differences between its types, we have to first get along with Ingress and Ingress Controllers: why do we need them and how do they align to support each other in the kubernetes cluster.

Let us consider an application named <b>"App"</b> deployed on Kubernetes cluster with 2 number of replicas which run on 2 different Worker nodes inside the cluster as shown below:

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/1.drawio.png" width=500>
<p align="center"><b>Fig:</b> Ingress in detail</p>

Now, in order to make our App accessible publicly, we have to create a Service of type Nodeport. Whenever we try to create a service for our application and if the IP address of atleast one of the  worker nodes and port number which is defined as part of the service is known, we can easily access our application from outside. This kind of service is referred to as 🔎 <b>External service<sup>1</sup></b> i.e. we can access our application outside the kubernetes cluster. Once this service receives the request from multiple clients, it will do the 🔎 <b>loadbalancing<sup>2</sup></b> and forwards the request to the application pods running on the 2 worker nodes.
Whenever we define our service with ***-type=NodePort*** , that particular service will be created as ***External Service***.

🟢 It is ***good*** for Development and Testing. 🔴 But not for Production application because there might be high chances that on eof our nodes might be crashed or the complete Kubernetes cluster might crash. In that case, the IP address of nodes and port number might change and additionally, we also do not want to share teh IP address  of worker nodes and port numbers to the clients. Thus, here again we have to create an 🔎 <b>Internal service</b>.

In order to create Internal service, we need to :

1. Define the service type as ***ClusterIP***.
2. This is available only inside the kubernetes cluster and the applications inside that kubernetes cluster can only access our service.
3. This means, it is 🔴not ***public***.
4. Also, it is to be kept in mind that providing the access to the service directly to the client is not recommended due to security reasons.


💡<b>NOTE:</b> 🔦
>In order to make our services secure, we can create service as an ***Internal service*** by defining type as ***ClusterIP***  so that the service is not accessible outside the cluster.
>Once we create service as ***Internal service***, then our clients cannot access our service using IP address and port number.

The goal to provide our our clients the access to our application in a secured way is where the term ***Ingress*** comes into the play.

In Kubernetes terminology, we call it as an ***Ingress Resource/Ingress Rule***. We can create an Ingress Resource by defining an YAML file similar to defining a service. Here is an overview of content of Ingress YAML file. We define:

* ***"kind"*** as Ingress and 
* The important part is under ***"specs"*** or ***specifications*** where we define the set of routing rules with 
 * ***host*** as - Domain name. This using this domain name, the clients can access the application. Domain name is assigned to the ***host*** attribute and 
 
Ingress resource alone cannot do all these job and needs an ***Ingress Controller*** which is basically an incrementation of teh Ingress resource. Ingress controller can be simply defined as a pod which is running on one of the worker nodes in our kubernetes cluster. We caan define the ingress Controller as a ***Daemon set*** or ***Deployment***. If the ingress controller is defined as:

| Daemon Set    | Deployment    |
| ------------- | ------------- |
|<ul><li>A DaemonSet ensures that all (or some) Nodes run a copy of a Pod.</li> <li>A DaemonSet schedules exactly one type of Pod per cluster node, masters included, thus when the ingress controller is defined using DaemonSet, the controllers are created on each worker node.</li> <p>Eg: If we have 6 worker nodes for our application, then Ingress controller will be created on 6 worker nodes.</p></ul>   | <ul><li>In a reasonably large cluster deploying ingress as deployment with multiple replicas is suitable compared to Daemonset.</li><li></li>Sometimes if the ingress-controller is running via a deployment and one of the nodes didn't have the pod allocated then the node could be listed as "unhealthy" in the loadbalancer healthcheck. </ul>  |



### 1.6 Routing in Ingress

One of the most important necessities of ingress controllers is that it supports both <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/5.1_Basic_properties_of_underlying_implementations.md#path-based-routing">Path-based</a> and <a href="">Host-based loadbalancing </a> and SSL termination. In this project, mostly host-based loadbalancing is used. Moreover, the L7 loadbalancing supports only HTTP and HTTPS rules for directing the traffic and hence they listen on port numbers 80 and 443 respectively.

#### Path-based routing:

* No host is specified here. Thus, the rule is applicable to all the inbound HTTP(S) traffic through the ingress controller.
* <code>PathType</code> field is required and specifies one of the three possible ways of interpretation of ingress object path mainly: ***"Exact"*** , ***"Prefix"***.
  
* <code>Exact</code> - matches the URL path exactly and with case sensitivity.
  
* <code>Prefix</code> - here the matches are based on a URL path prefix split by /. Moreover, the matches are case-sensitive too liek in Exact type and is done on path element-by-element basis.

* Let an user try to access a URL "www.example.com" with a particular path "/video".
* The Ingress Controller will understand the /video part and it will check the backend services for this /video. This is achieved because we specify a path and its backend in the ingress.yaml file as specified below:

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/Path-based_routing.drawio.png">


------------------------------------------------------------------------------------------------------------------------------------------

# Ingress Security


### 2.1 Introduction

Though Kubernetes can be considered to be the most useful orchestration tool in today's world, but to secure the applications running inside it is also a crucial topic to be thought about. Exposing an application to internet brings multiple ways of cyber threats and compromises thereafter affecting our application running.

Well, the most appropriate solution is ***TLS***. 

Kubernetes too allows TLS but we need certificates to use the TLS. As per the <a href="https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/">Official document of Kubernetes</a>, 

K8s provides an API  ***certificates.k8s.io*** which enables us to provision TLS certificates signed by a Certificate Authority (CA) that you control. These CA and certificates can be used by your workloads to establish trust.
***certificates.k8s.io*** API uses a protocol that is similar to the [ACME draft](https://github.com/ietf-wg-acme/acme/).
> <b>***Note:***</b> Certificates created using the certificates.k8s.io API are signed by a [dedicated CA](https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/#configuring-your-cluster-to-provide-signing). It is possible to configure your cluster to use the cluster root CA for this purpose, but you should never rely on this. Do not assume that these certificates will validate against the cluster root CA.


### 2.2 TLS and its working principle


Acronym for Transport Layer Security, it is a protocol to make privacy and data exchange security while communication with the Internet very easier. A very common use-case for TLS would be encrypting the communication between a web application and a server most commonly like a web browser loading any website for instance. 

The process running behind the scenes in order to secure our communication between the server and browser is known as "TLS/SSL handshake" which basically protects information while in transfer between the two aforementioned parties and authenticate the web application's identity as well ensuring the users that they are connecting to a legitimate application owner. In the TLS/SSL handshake method:

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/TLS_handshake.drawio.png">

* Each TLS certificate consists of key-pair namely:
  
     * Private-key
     * Public-key

* This means, whenever we visit any website, there is always a communication between the client server and the user's browser in order to ensure that there exists a secured TLS/SSL encrypted connection. Now, herewith there are two scenarios:


TLS certificates are also commonly termed as SSL(Secure Socket Layer). The term "TLS" refers to two main components namely:

* Certificate

* Private Key


Certificates are key to a functioning of a secure kubernetes cluster enabling the kubernetes components to perform mutual authentication. The fundamental working principle of a certificate in kubernetes functions in the following manner:

* Kubernetes offers an API to request/generate certificates named as <code>certificates.k8s.io/v1beta1</code>

* Clients create a certificate signing request (CSR) and send it to the API server.

* The user who is requesting the certificate is stored as part of the CSR resource.

* This CSR remains in a pending state unless and until a cluster CA approves it.
 
* Once this CSR request is approved, the certificate is issued.

-------------------------------------------------------------------------------------------------------------------------------------------
##### :bulb: ***NOTE***
>#***Cert-manager:***
>Now, since we know that we need a certificate for TLS, the next step engrosses ***how*** and ***where*** to manage them. This is where ***Cert-manager*** comes into picture.
>Cert-manager as the name suggest is a certificate-management tool which can work with Kubernetes. It can handle the opertaions like >Obtaining, renewing and using the SSL/TLS certificate. It is able to communicate with many Certificate Authorities like Lets Encrypt, HashiCorp Vault etc. and automatically issue the certificate which is valid. Additionall, it is also able to renew the certificates which are about to expire or in short we call it ***Certifcate Rotation***.
>***Cainjector in Cert-manager:***
>It is a command-line tool which helps in configuring the CA certificates for:
>[Mutating Webhooks](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#mutatingadmissionwebhook):
>1. They allow to modify any Kubernetes resource (Eg:pod) request.
>2. Mutating Webhooks can modify the incoming objects. 
>3. Mutating Webhooks can do it by sending a patch in the response. Examples of mutating webhooks are, adding additional labels and annotations, injecting sidecar containers, etc.
>[Validating Webhooks](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#validatingadmissionwebhook):
>1. This webhook calls any validating webhooks which match the request. 
>2. Matching webhooks are called in parallel; if any of them rejects the request, the request fails. 
>3. This admission controller only runs in the validation phase; 
>* [Conversion Webhooks](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definition-versioning/#webhook-conversion):
>1. Versions can have different schemas, and conversion webhooks can convert custom resources between versions.
>* [API Services](https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/api-service-v1/):
>1. APIService represents a server for a particular GroupVersion.

##### :bulb: ***NOTE***
>Wildcard certificates are the SSL certificates which are used to encrypt a domain and all its sub-domains as shown in the figure below:
><img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Research_Question/1.%20Ingress/wildcard-certificate-unlimited-subdomains.png" width=400>
><p>Fig: Wildcard certifcate. Source: <a href="https://comodosslstore.com/resources/how-do-wildcard-ssl-certificates-work/">Click here</a></p>
>These type of certificates consists of an asterisk ( * ) and a period before the domain name. The idea of extending a single certificate to encrypt all its sub-domains also enables us to reduce the cost and making the administration a lot simple and easy.
>But wildcard certificates do have an advanatage that if we want to cancel the certificate on any of the domain, then we have to cancel it in all the sub-domaains.
>Here i sthe link to the references followed: 
>* <a href="https://www.techtarget.com/searchsecurity/definition/wildcard-certificate">Wildcard certificate</a>
>* <a href="https://comodosslstore.com/resources/how-do-wildcard-ssl-certificates-work/">Working of Wildcard certificate</a>

 
>***NOTE***
>- Attention is to be paid that we do not want any random signed certificate here and that these certificates will be signed by the cluster Certificate Authority making sure that every single component in the cluster trusts this CA which further implies that they are going to trust any certificate generated through this API. Thus the CSR has to be approved for the certificate to be issued.


### 2.3 Automatic Certificate generation

Here we focus to generate the certificates automatically using cert-manager integrating wwith Lets Encrypt in order to ensure a smooth generation and auto-renewal whenever the certificates approach towards their expiry.

 
#### <ins>Cert-manager:</ins>
 
It is a kubernetes-native ccrtificate management controller consisting of <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#customresourcedefinitions-">CustomResourceDefinitions *</a> in order to configure the certificate authority and obtaining the certificates.
 

##### CustomResourceDefinitions *:

CRDs in short can be considered as some blocks of data which provides a technique to create, store and expose kubernetes API objects containing data but they taake no action sby themselves. Thus, in order to add some functionality or purpose to their usage, controller sor operaators are appended to them. The API server is a component of the Kubernetes control plane that exposes the Kubernetes API. The API server is the front end for the Kubernetes control plane. CRD operations, however, are handled inside kube-apiserver by the apiextensions-apiserver module. This is not a separate process, but a module integrated into kube-apiserver.
 
In terms of kubernetes, they can be defined as extension to kubernetes API which are not necessarily available while the default installation of kubernetes. It aims to define new aand unique object types like pods, replica-sets, configmaps, ingresses and enable us to manage them using the built-in kubernetes command-line-interface "kubectl".

The beneficial perspective that cert-manager withstands to auto-generate the certificates in kubernetes undergoes the process mentioned below.
 
<kbd><img src="https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/cert%2Blets-encrypt.png"></kbd>

<p align="center">Fig: Workflow of cert-manager</p>
<p align="center">Source: <a href="https://cert-manager.io/docs/">Click here</a></p>
 
   Process:
   
   1. 



### 3. Authentication and authorization of kubernetes applications
