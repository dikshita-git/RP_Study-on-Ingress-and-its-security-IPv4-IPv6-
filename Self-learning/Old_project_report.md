## Title

Granular access control to kube-apisrever using OpenID Connect 

## Topic

This project aims to improve the security of kube-apiserver on the basis of token-based authentication protocol called OpenID Connect.

## Team

***Supervisor:*** Prof. Dr Martin Leischner

***Mentor:*** Richard Clauß

***Candidate:*** Dikshita Kalita


## Table of Contents:

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#1-introduction">1. Introduction</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#2-approach-and-goal">2. Approach and Goal</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#3-research-questions">3. Research Questions</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#4-fundamentals">4. Fundamentals</a>
  
* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#41-ingress">4.1 Ingress</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#42-security-in-kubernetes">4.2 Security in kubernetes</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#421-security-issues">4.2.1 Security issues</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#422-classification-of-kubernetes-security-solutions">4.2.2 Classification of security solutions</a>
    
* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#4221-securing-the-kubernetes-hosts">4.2.2.1 Securing kubernetes hosts</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#4222-securing-the-kubernetes-components">4.2.2.2 Securing kubernetes components</a>

    * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#i-controlling-network-access">i) Controlling network access</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#4223-limit-direct-access-to-nodes">4.2.2.3 Limit direct access to Nodes</a>

    * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#i-limit-access-to-etcd">i) Limit access to etcd</a>

    * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#ii-limit-direct-access-to-kube-api-server">ii) Limit direct access to kube-API server</a>

        * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#blue_square-authentication">Authentication </a>

        * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#blue_square-authorization">Authorization </a>

        * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#blue_square-admission-control">Admission control </a>

    * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#iii-using-tls">iii) Using TLS</a>


    * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#iv-control-access-to-the-kubelet">iv) Control access to Kubelet</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#5-methodology">5. Methodology</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#6-results">6. Results</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#7-state-of-the-art">7. State of the art</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#8-summary">8. Summary</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#9-literatures">9. Literatures</a>




----------------------------------------------------


## 1. Introduction

The word Ingress describes traffic originating from sources external to the network under investigation. In the kubernetes terminology, it is the name of a special resource type or more specifically a kind of an API object which follows some routing rules defined by the user to direct and forward traffic from the external world to the services running in the kubernetes cluster. Ingress has a strong precedence over NodePort or Loadbalancer in a way that it can integrate the routing rules in a single resource instead of exposing multiple services available for a particular application individually. It defines routing rules on layer 7 for the protocols HTTP and HTTPS. Other protocols are not supported. Additional features are loadbalancing, path-based routing, SSL termination, etc. . These features are implemented by dedicated ingress controllers. Since only HTTP and HTTPS rules are supported they usually listen on port numbers 80 and 443 respectively. There are several variations of ingress controllers available which can also be categorized as ***cloud specific ingress controllers*** which is intended to integrate with the specific cloud loadbalancers and open ***source ingress controllers*** which is defined for more generic uses other than clouds. Some of the most popular and widely used implementations of ingress controllers are Nginx, Haproxy and Traefik. 

Additionally, it also becomes crucial to secure the ingress layer so that only the valid and legitimate traffic enters while the malicious and unauthorised traffic can be discarded or prevented. This also appends with the authentication of the application srunning inside the cluster to which the traffic is intended to. With smaller application which do not uphold frequent releases, this common non-functional requirement can be adjusted wwith manageable amount of complexity. However, the scenario alters with applications having very fast releasing velocities. 

This project aims to compare the differences between the most widely used ingress controllers. Moreover, it also anaylses various modes of authentication and authorization plugins which could be used to limit the access of unauthorized users or traffic in order to secure the kubernetes cluster.

In order to proceed further with the topic, the project pivots the belowmentioned points of motivation:

* Analyse points of diferences between three-most variations of open-source ingress controllers.

* Determine possible prospects to limit the access to the kubernetes application and integration with the identity providers ensuring a secured administrataion of the cluster.


--------------------------------------------------

## 2. Approach and Goal

This project aims to secure ingress in a K3s cluster by investigating the varied methods of certificate generation using the three most popular implementations of open-source igress cntrollers namely Traefik, HaProxy, Nginx. IN addition to this, it investigates various modes of kubernetes authentication and authorization modes in order to limit the user access to the cluster and its components ensuring its solid security.
Further, in order to gain more insights in the described functionalities, some research questions shall be conducted which will be answered in the course of the project on the basis of research and demonstrations.

---------------------------------------------


## 3. Research Questions

- 3.1 Basic properties of underlying implementations
  
     - a) What are the principal point of differences between Traefik, HaProxy and Nginx?

- 3.2 Security in kubernetes
     
     - a) Analyse best modes to authenticate users accessing the cluster and its resources.
     
     - b) Compare their key differences based on scenarios.     
---------------------------------------


## 4. Fundamentals

## 4.1 Ingress:


Ingress is the object that allows access to the Kubernetes services from outside of the Kubernetes cluster. It is basically used when we have multiple services in our K8s cluster and we want the user requests to be routed to the service based on their path. In other words, K8s ingress is a amalgamation of routing rules that control how the external users can/should access the services running in a K8s cluster typically via HTTP/HTTPS (operate at the application layer of the network stack) through a single externally reachable IP address. The figure below depicts the two main components of an ingress which play a crucial role in its implementation in a cluster.

<kbd>![ingress_components](https://github.com/dikshita-git/Research-Project/raw/main/Wiki-page-images/Others/Ingress_new_components.png)</kbd>
<p><i>Fig: Ingress components</i></p>

### a) Ingress controllers:

1) Kubernetes has a built-in feature to connect different services of the application together using built-in service discovery and load-balancing and it also supports stateful applications. But for the final goal of delivering the applications to the users, we need an L7 Loadbalancer or Ingress Controller in other terms.

2) They are similar to pods just like any other applications. Hence they are part of cluster and can see other pods too.

3) One of the most important necessities of ingress controllers is that it supports both [Path-based](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#path-based-routing) and [Host-based loadbalancing ](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#host-based-routing)and SSL termination.

3) Ingress resources require an Ingress Controller component to run as a Layer 7 proxy service inside the cluster.

4) Once we have deployed the pod for the Ingress Controller, the Kubernetes rule says that the pod cannot be directly exposed. It has to have a service. This implies that we have an ***"Ingress service"*** created which is basically a ***"NodePort"*** so that the services can be accessed by the outside world.
A service should be a ClusterIP so that the services cannot be accessed directly but only be accessed via the Ingress.
Hence, in the Fig Ingress_2 we define the services as ClusterIP(means they cannot be accessed from outside of the Internet) but since the Ingress Controller can access these services and Ingress Controller itself is exposed, thus we get to see or access the services.
Once the Ingress Controller is created, we have to define the rules separately as they are not defined inside the Ingress Controller .

5) The controller has the ability to inspect HTTP requests and thus directs a client to the correct pod based on URL path or domain name it finds.

<kbd>![ingress_rule_eg](https://github.com/dikshita-git/Research-Project/raw/main/Wiki-page-images/Others/New_Ingress_working_principle.png)</kbd>
<p><i>Fig: Working principle of ingress</i></p>

In the picture above,

  i. An User sends or visits an URL in the browser, that gets redirected to the Ingress Service

  ii. Ingress Service then forwards the request to Ingress Controller

  iii. Ingress Controller then checks the Ingress rules and based on these rules, it takes a decision to.


<kbd>![ingress_service](https://github.com/dikshita-git/Research-Project/raw/main/Wiki-page-images/Others/Ingress_intro.png)</kbd>
<p><i>Fig: Exposing multiple services using ingress. Source: <a href="https://www.manning.com/books/kubernetes-in-action-second-edition">Kubernetes in action by Marko Lukša</a></i></p>

In the figure above,

   i. There is only one entrypoint in ingress(2nd block) that all traffic goes through.

   ii. Each service is connected to 1 ingress controller, which in turn is connected to many internal pods.


##### Path-based routing:
No host is specified here. Thus, the rule is applicable to all the inbound HTTP(S) traffic through the ingress controller.

* PathType field is required and specifies one of the three possible ways of interpretation of ingress object path mainly: "Exact" , "Prefix".

* Exact - matches the URL path exactly and with case sensitivity.

* Prefix - here the matches are based on a URL path prefix split by /. Moreover, the matches are case-sensitive too liek in Exact type and is done on path element-by-element basis.


##### Host-based routing:
Here instead of having a path, we have a proper domain name.
Thus if a request comes from a particular DNS name like [dkrp2.xyz](https://dkrp2.xyz/), the traffic gets further forwarded to backend services


### b) Ingress API object

This API object is meant to define the routes and mapping to the external traffic in order to access the services inside the cluster. It also provide SSL termination, loadbalancing and <a href="">name-based virtual hostingy</a>.

##### Name-based virtual hosting:

It is a method to host multiple domain names in a single or a collection of servers and uses the host name given by the client. This host name is given by the protocol which is used. Name-based virtual hosting thus also involves an advantage of saving IP addresses.


## 4.1.1 Types of ingress controllers:

There are many types supported by kubernetes but the most popular ones are listed below:

1. Nginx

2. AWS

3. GCE

4. Ha-proxy

5. Traefik

6. Istio ingress

7. EnRoute

8. Contour

9. Kong

------------------------------------------

## 4.2 Security in kubernetes:

It is prominent that kubernetes provide a very strong layer of security, however certain conditions of vulnerabilities like malwares in the containers running in the cluster, broken container images used, compromised or rogued user along with some attack forces are highly possible and should always be taken under consideration. This implies to undertake measures to secure kubernetes as a whole. 

In an overall SDLC, kubernetes security checks can be carried out in the area shown in the figure below:

<kbd>![](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/Kubernetes-security-architecture.png)
<p><i>Fig: Implementational are of kubernetes security in SDLC. Source: <a href="https://snyk.io/learn/kubernetes-security/">Kubernetes security by Snyk</a></i></p></kbd>



### 4.2.1 Security issues:

In order to deploy our applications in a secured and hazzle-free manner, it is highly recommended to analyse the security issue. Though there could be enormous areas of susceptibilities while a deployment, however the most crucial criterias that are very likely are listed below:

| Access controls (Infrastructure level)  | Improper credential management | Broken container images| Runtime threats|
| ---------------- | ------------------------------------------- |-------------------------------------- | --------------------------- |
| <ul><li>This issue arises when an unauthorization user gets access to the kubernetes resources.</li><li>Attackers always try to breach and escalate privileges by gaining unauthorized access in order to steal the data.</li><li>***Eg:***<p> Let us consider kubernetes's own Role-Based Access Control (RBAC) with two users:</p> <ul><li>A: is given ***pod-reader/only read*** access to the pods running in the kubernetes cluster.</li><li>B: is given ***pod-writer/write*** access (adding or deleting or modifying pods) to the pods running in the kubernetes cluster. </li></ul></li>The problem arises if A manages to get the privileges of B using ***privilege escalation attack***.</li></ul>    |    <ul><li>Kubernetes secrets by default are stored in an unencrypted form in the underlying database of kube API-server named as "etcd".</li><li>This implies that built-in security in kubernetes secret-management can be very highly exploitable and compromised.</li><li>Anyone with API access can retrieve or modify a Secret by accessing the etcd. Additionally, anyone who is authorized to create a Pod in a namespace can use that access to read any Secret in that namespace</li></ul>     |      <ul><li>Conatiner images are a storehouse of system libraries,  configuration, container engines etc. and are static in nature. The registries even Docker registry for that matter are never completely attack-free and are prone to severe vulnerabilities leading to container break-out and breach of the cluster as a whole. </li><li>If these containers are launched, then its an enormous security issue for the whole cluster.</li></ul>     |   <ul><li>This involves irregular and weak auditing and monitoring of the running containers .</li><li>Any misconfigurations in the RBAC policy, infected containers etc. are adequate enough for the cluster to skate on thin ice.</li></ul>              |

This is table documented is an idea-driven and referenced from: <a href="https://kubernetes.io/docs/concepts/configuration/secret/">Kubernetes secret</a>, <a href="https://www.appsecengineer.com/blog/top-kubernetes-security-issues-and-how-to-fix-them">Kubernetes security issues and how to fix them</a>



### 4.2.2 Classification of kubernetes security solutions:

Security applications in kubernetes can be further cumulatively classified into three types as shown in the figure below:


<kbd>![Types of security in kubernetes](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/security1.drawio.png)</kbd>
<p><i>Fig: Possible type of security solution in kubernetes</i></p>


### 4.2.2.1 Securing the kubernetes hosts:

Kubernetes runs on bare-metal, on-premise or cloud but it always consists of an underlying host where it is running. This is also the case while considering master node which is a host that runs kube API-server along with other master plane components and worker nodes as the host running kubelet along with kube-proxy. These hosts at all times should consist of an updated and hardened operating system, lastest version of kubernetes, vital firewall rules and be able to undertake specific constructive security solutions depending on the environment.



### 4.2.2.2 Securing the kubernetes components:

#### i) Controlling network access:

Authentication and authoriziation of the cluster nodes and cluster itself is intensely recommended as kubernetes consists of many distinctive ports responsible for its components which are extensively well-defined. This property exposes itself as a vulnerability as they are very easy to detect and hence to attack. The table below depicts the distinctive ports in kubernetes as aforementioned. 

<kbd>![](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/ports.png)</kbd>
<p><i>Fig: Default ports in kubernetes. Source: <a href="https://kubernetes.io/docs/reference/ports-and-protocols/">Ports and protocols</a></i></p>

In order to secure them, the network must block the access to these ports and limit the access to the kube API-server with allowance to only the trusted networks.


### 4.2.2.3 Limit direct access to nodes

In order to ensure restricted user access to the resources, we can use kubernetes authorizaton plugins. This would enable us to implement a fine-tuned control rules for containers, namespaces etc. 


#### i) Limit access to etcd

Datas are highly crucial discrete values and in kubernetes, all the secrets, state informations, network and storage configurations are stored in its default database "etcd". Though kubernetes v1.7 introduced encryption of secrets but it is to be knwon that it doesn't enable it by default. Even if it is set to default, the ***Data Encryption Key (DEK)*** is stored on the same node were the secrets are also stored. This implies that any access to the node would enable us to bypass the encryption. 
However, kubernetes 1.11 enabled a beta feature that lets you store ***Key Encryption Keys (KEK)*** outside of our Kubernetes cluster. These types of keys are used to encrypt and decrypt data encryption keys and should be safely guarded. Other alternative under consideration would be:  Hardware Security Modules (HSM) or cloud-based Key Management Stores (KMS) for storing your key encryption keys.

#### ii) Limit direct access to kube-API server

Kubernetes is an API-centric tool and this API is served by the kube-API server. A typical flow of secured API request through all the standard checks would look like the figure below:

<kbd>![](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/api.png)</kbd>
<p><i>Fig: Flow of API request. Source: <a href="https://nigelpoulton.com/books/">The Kubernetes book by Nigel Poulton </a></i></p>

The figure above picturizes:

a) An user trying to create a Deployment called “dev” in the “whoami” Namespace.

b) User issues a kubectl command to create the Deployment. This generates a request to the API server with the user’s credentials embedded. 

c) Due to the presence of  TLS, the connection between the client and the API server is secure. 

d) The authentication module determines whether it’s the user or an imposter. After that, the API security and RBAC authorization module (RBAC) determines if the user is allowed to create Deployments in the whoami Namespace. If the request passes authentication and authorization, admission control checks and applies policies, then the request is finally accepted and executed.

This scenario can also be illustrated with the figure below:

<kbd>![](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/api-official-new.png)</kbd>
<p><i>Fig: API request through security checks. Source: <a href="https://kubernetes.io/docs/concepts/security/controlling-access/">Controlling access</a></i></p>


##### :blue_square: Authentication:

It basically defined as proving our identity using credentials. Referring to the <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#ii-limit-direct-access-to-kube-api-server">Flow of API request</a>, we infer that once the TLS connection gets established, the request moves forward to the authentication step. All requests to the API server have to include credentials, and
the authentication layer is responsible for verifying them. If verification fails, the API server returns an HTTP 401 and the request is denied. If it passes, it moves on to authorization. The input to the authentication step is the entire HTTP request; however, it typically examines the headers and/or client certificate.

Kubernetes uses bearer tokens, client certificates and authentication proxy in order to authenticate the incoming API request with the help of authentication plugins. This incoming HTTP request is made to the kube-API server and the plugins then associates the request with the following mentioned attributes:

* <b>Username:</b> A string which identifies the end user. Common values might be kube-admin or masterproject@example.com.

* <b>UID:</b> a string which identifies the end user and attempts to be more consistent and unique than username.

* <b>Groups:</b> a set of strings, each of which indicates the user's membership in a named logical collection of users. Common values might be system:masters or devops-team.


* <b>Extra fields:</b> a map of strings to list of strings which holds additional information authorizers may find useful.

##### There are various kubernetes built-in API authentication process which are suitable only for ***smaller clusters*** or at the staging level which are intended to implement only specific authentication methods. Thus they can also be regarded as ***closed-end*** authentication plugins.

| Kubernetes built-in authentication methods for smaller clusters  | Description       |
| -------------------------------------  | --------------------------------------------------------------- |
| Static token file / Static passsword file:        | <ul><li>It is a type of method in which the administrator defines a set of credentials for users in a password.csv file without defining or configuring any validity period. Best for beginner in kubernetes.</li><li>This password.csv consists of password, username and userID,followed by optional group names respectively, for the users we want to authenticate. <p>password1,user_1,user1_ID,"group1,group2,group3"</p><p>password2,user_2,user2_ID,"group1,group2,group3"</p></li><li>This file ensures that only authenticated users are allowed access to the API server</li><li>Static token is a mechanism where the cluster administrator generates arbitrary strings called "tokens" and assign them to the users of the API.</li><li>The API server reads the bearer tokens from the token file when <code>-token-auth-file=SOMEFILE </code> option is given on the command line.</li><p><b>:red_square:Disadvantage:</b></p><li>This token lasts upto indefinite time.</li><li>Any changes in the token file can only be executed if the kube API server is restarted.</li><li>Additionally, this file should be manually modified every time for any changes in its data.</li></ul> |
| X509 Client-Certs                      | <ul><li>It is a public-key in a private/public key pair.</li><li>Simplest method to authenticate.</li><li>X.509 certificates bind an identity to a public key using a digital signature. </li><li>We must first create a private key and a Certificate Signing Request, or CSR, for the user account you want to authenticate for using this authentication strategy.</li> <p>:red_square: <b>Disadvantages:</b></p> <ul><li>Though it is a secure way to access the kubernetes cluster, but it has a biggest disadvantage that the certificates isssued by the cluster cannot be revoked. In other words, if this issued certificate gets stolen, it will still remain active until it expires by its expiry date or unless the adminsitrator replaces the entire CA in the cluster.</li></li></ul></ul><p>:green_square: <b>Suitable solution to reduce risk:</b></p> Issuing certificate with shorter validation dates. |
| Bootstrap Tokens                       | <ul><li>These are 23-character bearer tokens which are stored as secrets in the kubernetes cluster which are then issued to individual kubelets and are meant to be used when creating the cluster or while joining nodes in the cluster.</li><li>These Secrets are then read by the Bootstrap Authenticator in the API Server. </li><li>Expired tokens are removed with the TokenCleaner controller in the Controller Manager. </li><li>Consists two distinct parts separated by a dot: <code>{token-id}</code>.<code>{token-secret}</code> : <ul><li><code>{token-id}</code>, with the size of six characters, is considered the public information of the key that identifies the token and used when referring to a token without leaking the secret part used for authentication. </li><li><code>{token-secret}</code> acts as the secret and should only be shared with trusted parties.</li></ul> </li></ul> <p><b>:red_square: Disadvantage:</b></p><p>No expiration dates.</p>|
| Service account tokens                 | <ul><li>A service account is a Kubernetes resources, created and managed using the Kubernetes API, meant to be used by in-cluster Kubernetes-created entities, such as Pods and is automatically enabled authenticator by the kube API server and uses signed bearer tokens to verify requests to the Kubernetes API server.</li><li>They are tied to a specific namespace and are used to achieve specific Kubernetes management purposes, they should be carefully and promptly audited for security.</li><li>These service accounts are associated with the pods running in the cluster through the <code>ServiceAccount</code> Admission Controller.</li></ul> |


Kubernetes supports external authentication plugins which are suitable for ***larger clusters**** or at the production level. These plugins rather provide point of extensibility in order to integrate custom authentication logic. Thus they can be regarded as ***open-end*** plugins. 

| Authentication methods for larger clusters/production  | Description       |
| -------------------------------------  | --------------------------------- |
| Open ID Connect                        | <ul><li>Since the default authentication method of kube-API server in kubernetes is limited to simple use-cases with minimal people manage the cluster, thus resulting in :red_square: ***no handling of user management***, hence it has to rely on external plugins. This leads to Open ID connect as it can manage users and groups which integrates very well with the kubernetes RBAC.</li><li>Open ID connect is a simple open authentication protocol that sits on top of the <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#oauth-20">OAuth 2.0 *</a> protocol.</li><li>It allows the clients of all types, including web-based, mobile, and JavaScript clients in order to receive authenticated sessions and verify its end-users on the basis of authentication methods performed by the authentication servers and also to obtain profile informations about the end-users in an interoperable and REST-like manner. </li>It operates authentication by allowing users to use single sign-on to authenticate their identities.</li><li>It enables us to externalize the authentication process, use short-lived tokens, and leverage centralized groups for authorization.</li><li>It adds an additional token called ***ID Token***.</li><li>Working: <p>![](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/openIDconnect.png)</p><p align="center">Fig: Workflow of OpenID connect</p><ul><li>Login to your identity provider</li><li>Your identity provider will provide you with an <code>access_token</code>, <code>id_token</code> and a <code>refresh_token</code>.</li><li>When using kubectl, use your <code>id_token</code> with the <code>--token flag</code> or add it directly to your <code>kubeconfig</code>.</li><li><code>kubectl</code> sends your <code>id_token</code> in a header called Authorization to the API server.</li><li>The API server will make sure the JWT signature is valid by checking against the certificate named in the configuration</li><li>Check to make sure the <code>id_token</code> hasn't expired.</li><li>Make sure the user is authorized.</li><li>Once authorized the API server returns a response to <code>kubectl</code></li><li><code>kubectl</code> provides feedback to the user.</li> :bulb: NOTE: To identify the user, the authenticator uses the id_token (not the access_token) from the OAuth2 token response as a bearer token.</ul></li></ul> |
| Webhooks                               | <ul><li>Webhooks use authentication in order to integrate themselves with their destination systems and to sign the secrets to verify the integrity  of their requests in a secured manner.</li><li>In kubernetes, webhooks support ***bearer token authentication*** and should be used over HTTPS(TLS).<p>:bulb: NOTE: Bearer tokens are opaque strings, and they're the predominant type of access token used with OAuth 2.0.</p></li><li>It is a hook to verify the bearer tokens.</li><li>When a client attempts to authenticate with the API server using a bearer token, the authentication webhook POSTs a JSON-serialized <code>TokenReview</code> object containing the token to the remote service.</li><li>The Kubernetes API server defaults to sending <code>authentication.k8s.io/v1beta1</code> token reviews for backwards compatibility. To opt into receiving authentication.k8s.io/v1 token reviews, the API server must be started with <code>--authentication-token-webhook-version=v1</code>.</li><li>Using webhook authentication method, users get the flexibility to authenticate through kube-API server using the tokens which they generated from external services like Github. Such authentication method is beneficial if we have authentication services for our other workloads which we want to use the same process with kubernetes too.</li></ul> |
| Authentication proxy                   | <ul><li>Kubernetes API server has the capability to identify its users on the basis of ***Request value headers*** such as <code>such as X-Remote-User</code>. </li><li>These request values are set by the authentication proxy.</li><li>Crucial informations such as namespaces, usernames,etc. can be retrieved from these headers which would assist us in order to find if the request is valid.</li><li>This method is convenient if we want data-based authentication stored in an external service.</li></ul> |



This table has references from: 

* Static token: <a href="https://kubernetes.io/docs/reference/access-authn-authz/authentication/#static-token-file">Static file token</a>, <a href="https://minikube.sigs.k8s.io/docs/tutorials/token-auth-file/">Using static token file</a>.

* X509 client-certs: <a href="https://security.stackexchange.com/questions/1438/what-is-the-difference-between-an-x-509-client-certificate-and-a-normal-ssl-ce">X509 certs</a>, <a href="https://en.wikipedia.org/wiki/X.509">X509 Wikipedia</a>.

* Service accounts: <a href="https://kubernetes.io/docs/reference/access-authn-authz/authentication/#service-account-tokens">Kubernetes service accounts</a>

* Open ID connect: <a href="https://kubernetes.io/docs/reference/access-authn-authz/authentication/#openid-connect-tokens">Opn ID conect token/a>

* Webhooks: <a href="https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication">Kubernetes webhooks</a>, <a href="https://developer.zendesk.com/documentation/event-connectors/webhooks/webhook-security-and-authentication/#:~:text=the%20API%20documentation.-,Webhook%20authentication,authentication%20property%20from%20the%20request.">WEbhook</a>

* Authentication Proxy: <a href="https://kubernetes.io/docs/reference/access-authn-authz/authentication/#authenticating-proxy">Authentication proxy</a>


###### OAuth 2.0:

It stands for “Open Authorization”, is a standard designed to allow a website or application to access resources hosted by other web apps on behalf of a user.

* OAuth 2.0 is an :green_book: ***authorization protocol*** and :red_square: ***NOT an authentication protocol***. 

* OAuth 2.0 uses Access Tokens. 

An Access Token is a piece of data that represents the authorization to access resources on behalf of the end-user. OAuth 2.0 doesn’t define a specific format for Access Tokens. However, in some contexts, the JSON Web Token (JWT) format is often used. This enables token issuers to include data in the token itself. Also, for security reasons, Access Tokens may have an expiration date.

* The prerequisites for a client before using OAuth is to acquire a ***client-ID*** and ***client-secret*** from the authorization server in order to identify and authenticate itself while it tries to get an Access Token.

While using OAuth 2.0, the client (websites, mobile apps, desktop applications etc.) initiates the access requests. This is followed by a workflow of token request, exchange and response which is illustrated in the figure below:

<kbd>![](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/oauth2.0_workflow.png)</kbd>
<p><i>Fig: Workflow of OAuth2.0. Source: <a href="https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2">Introduction to OAuth</a></i></p>

      1. Here, the client requests for an authorization from the user(resource owner).

      2. The client receives an authorization grant if the user authorizes the request successfully.

      3. The client requests for Access tokens or authentication codes from the authorization server(API) by providing his client-id and client-secret as his identification. Additionally, he also sends scopes and an endpoint-URI (redirect URI) to where the authorization server could send the Access tokens to. 

      4. If the client identities are authenticated and the authorization grant is valid and confirms that the scopes that are requested are permitted then authorization server (API) issues the Access token to the client. Here the process of authorization gets completed.

      5. With the help of the Access token received, the client now requests resources from the resource server(API) and provides the Access token received for authentication .

      6. If the Access token is valid, then resource server(API) gives the resource to the client.

Reference: <a href="https://auth0.com/intro-to-iam/what-is-oauth-2/">OAuth 2.0</a>


##### :blue_square: Authorization:


##### :blue_square: Admission control:


#### iii) Using TLS

Considering kubernetes to be secured by default, the fact that the communication between services in the cluster should always be secured using TLS which ensures traffic encryption by default.

The process running behind the scenes in order to secure our communication between the server and browser is known as "TLS/SSL handshake" which basically protects information while in transfer between the two aforementioned parties and authenticate the web application's identity as well ensuring the users that they are connecting to a legitimate application owner. In the TLS/SSL handshake method:

<kbd>![](https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/TLS_handshake.drawio.png)</kbd>
<p><i>Fig: TLS workflow. Reference: <a href="https://www.digicert.com/how-tls-ssl-certificates-work">TLS working</a></i></p>

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



#### iv) Control access to the kubelet




---------------------------------------


## 5. Methodology

As an approach to the research questions, firstly a k3s cluster with one master node is set up where two workers nodes were assigned to it. A detailed description of the approach is indicated below:

### Using Traefik ingress controller to automatise certificates : <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Demo/Certificate_with_k3s%2Btraefik">Codebase here</a>

This case includes using the default ingress controller of k3s "Traefik" in order to issue a wildcard certificate to our host name. For this, once the cluster is set up, Custom resource definitions are installed before installing cert-manager because cert-manager uses the custom resources to define the resources which the users interact with while using cert-manager like Certificates and Issuers. Once these are defined, we safely install cert-manager which is repsonsible for managing our certificates. After checking the kubernetes namespace using the command:

```
k3s kubectl get namespaces
```
We can confirm if our cert-manager is running in order to proceed to the next step of creating a ClusterIssuer file so that we get a CA signed certificate. Furthermore, a wildcard certifcate is requested for our domain by creating an YAML file called certificate which consists of secretName along with the domain name. As soon as the certificate is ready, we can use it with the ingress and the wildcard certificate generated will start working from Lets encrypt. We can check the same using the cocmmnad:

```
kubectl describe certificate -n kube-system
```

This will show the exact status of the certificate if it is created successfully or not and also a detailed view of the certificate generated.

## 6. Results

## This consists of the results to Research question : "Basic properties of underlying implementations" with the following sub-questions:

<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/3.1_Basic_properties_of_underlying_implementations.md#a-nginx-based-ingress-controller-vs-haproxy-based-ingress-controller-vs-traefik">a) What are the principal point of differences between Traefik, HaProxy and Nginx?</a>

***************************************************************************************************************************


## a) Nginx-based Ingress controller Vs HaProxy-based ingress controller Vs Traefik

Ingress in kubernetes can be implemented by using different ingress controllers like Nginx, Haproxy, Traefik, Istio, Envoy etc. Each ingress should specify a ***class*** which can be defined as a reference to an <code>IngressClass</code> resource which contains additional configurations including the name of the controller that should implement the class. 

💡<b>NOTE:</b> 🔦

> 🔴Without ingress class, the ingress controller will not evaluate the ingress rules.

Here is an example(see under annotations of the ingress.yaml):<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Demo/Certificate_with_k3s%2Bnginx/Ingress.yaml"><code>Click here</code></a> 

##### The section below includes a comparison table showing the focal point of differences between the mostly used opensource ingress controllers. 

|                      |Nginx-based Ingress controller      |    HaProxy-based ingress controller       | Traefik      |
|     :---:    |            :---:                    |     :---:                                 |  :---:        |
| Build on | nginx/nginx plus   | haproxy    | traefik    |
| Supported Protocols | <ul><li>http/https</li><li><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/3.1_Basic_properties_of_underlying_implementations.md#-http2">http2 * </a></li><li><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/3.1_Basic_properties_of_underlying_implementations.md#-grpc">grpc * </a></li><li>tcp/udp</li></ul>   | <ul><li>http/https</li><li><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/3.1_Basic_properties_of_underlying_implementations.md#-http2">http2 * </a></li><li><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/3.1_Basic_properties_of_underlying_implementations.md#-grpc">grpc * </a></li><li>tcp</li><li>tcp+tls</li></ul>     | <ul><li>http/https</li><li><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/3.1_Basic_properties_of_underlying_implementations.md#-http2">http2 * </a></li><li><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/3.1_Basic_properties_of_underlying_implementations.md#-grpc">grpc * </a></li><li> tcp</li><li>tcp+tls</li></ul>    |
| Algorithm of loadbalancing    |     <ul><li>Round-Robin</li><li>Hash</li><li>IP Hash</li><li>Hash random</li><li>Sticky sessions</li></ul>     | <ul><li>Round-Robin</li><li>URI</li><li>url_param</li><li>Sticky session</li><li>Header</li></ul>       | <ul><li>Weighted Round-Robin</li><li>Dynamic-Round-robin</li><li>Sticky session</li></ul>      |
| Traffic routing logic  |     <ul><li>Host</li><li>Path</li><li>Header</li><li>Method</li><li>Query param (all with <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/3.1_Basic_properties_of_underlying_implementations.md#-regex---regular-expression"> regex<sup> * </sup></a> expect host)</li></ul>     | <ul><li>Host</li><li>Path</li></ul>       | <ul><li>Host (<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/3.1_Basic_properties_of_underlying_implementations.md#-regex---regular-expression"> regex<sup> * </sup></a>)</li><li>Path (<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/3.1_Basic_properties_of_underlying_implementations.md#-regex---regular-expression"> regex<sup> * </sup></a>)</li><li>Headers(<a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Chapters/Results/3.1_Basic_properties_of_underlying_implementations.md#-regex---regular-expression"> regex<sup> * </sup></a>)</li><li>Query</li><li>Path Prefix</li><li>Method</li></ul>      |
| Authentication Protocols | <ul><li>Basic</li><li>Client cert</li><li>External OAuth</li></ul>   | <ul><li>Basic</li><li>OAuth</li><li>Auth-TLS</li></ul>      | <ul><li>Basic</li><li>Auth-url</li><li>Auth-TLS</li><li>External Auth</li></ul>     |
| Graphical user interface    |    Grafana can be used to check the metrics    | Grafana or Datadog can be used to check the metrics       | Traefik has its own dashboard included      |

##### * HTTP2

A binary protocol (revision of HTTP versions) which introduces the concept of full requests and response multiplexing. This led to achieving efficiency in terms of:

* Network Speed
* Security
* Reliability
* Compression
* Multiplexing

This is a summary derived from <a href="https://cheapsslsecurity.com/p/http2-vs-http1/">Reference</a>


##### * gRPC

* It is a modern lighweight communication protocol to connect services efficiently in and across the data centers with siupport for loadbaalncing, health-check, authentication and tracing.
* Based on protocol buffer which is basically an open source mechanism in order to serialize the structed data and is language and platform neutral.
* Works effectively with L7 or application layer proxy loadbalancing.

Here are the reference list: <a href="https://www.capitalone.com/tech/software-engineering/grpc-framework-for-microservices-communication/">gRPC framework</a>, <a href="https://learn.microsoft.com/en-us/aspnet/core/grpc/performance?view=aspnetcore-6.0">gRPC performance</a>


##### * Regex - Regular Expression

The ingress controller supports case insensitive regular expressions in the spec.rules.http.paths.path field. This can be enabled by setting the nginx.ingress.kubernetes.io/use-regex annotation to true (the default is false). Reference: <a href="https://kubernetes.github.io/ingress-nginx/user-guide/ingress-path-matching/">Read here</a>

The comparison table is inspired by: <a href="https://kubevious.io/blog/post/comparing-top-ingress-controllers-for-kubernetes">Read here</a>

After an investgation for the differences between variations of ingress controllers, my practical demonstration implemented with Traefik in order to auto-generate a certificate using cert-manager integrated with Lets Encrypt:

* <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Demo/Certificate_with_k3s+traefik/README.md"><code>Traefik</code></a>


--------------------------------

## This consists of the results to Research question : "Security in kubernetes" with the following sub-questions:

<a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#a-analyse-various-modes-to-authenticate-and-authorize-running-applications-inside-the-cluster">a) Analyse various modes to authenticate and authorize running applications inside the cluster.</a>

<a href="">b) How can certificates be automatically generated and auto-rotated to secure the ingress with ease?</a>


## a) Analyse various modes to authenticate and authorize running applications inside the cluster

## b) How can certificates be automatically generated and auto-rotated to secure the ingress with ease?

------------------------

## 7. State of the art

------------------------

## 8. Summary


------------------------

## 9. Literatures


<details><summary>Ingress</summary><p>
  
  * <code><a href="https://kubernetes.io/docs/concepts/services-networking/ingress/">Ingress</code></a>
  * <code><a href="https://opensource.googleblog.com/2020/09/kubernetes-ingress-goes-ga.html">Ingress routing types</a></code>
  

</p></details>

<details><summary>TLS</summary><p>
  
  * <code><a href="https://opensource.com/article/20/3/ssl-letsencrypt-k3s">TLS in K3s</a></code>
  
  * <code><a href="https://www.thebookofjoel.com/k3s-cert-manager-letsencrypt">TLS in K3s with traefik, cert-manager and Lets-Encrypt</a></code>
  
  * <code><a href="https://sysadmins.co.za/https-using-letsencrypt-and-traefik-with-k3s/">HTTPS using Letsencrypt and Traefik with k3s</a></code>
  
  * <code><a href="https://devopscube.com/configure-ingress-tls-kubernetes/">How To Configure Ingress TLS/SSL Certificates in Kubernetes</a></code>
  
  * <code><a href="https://lachlan.io/blog/using-wildcard-certificates-with-traefik-and-k3s">Using Wildcard Certificates with Traefik and K3s</a></code> 
  
</p></details>

<details><summary>Cert-manager</summary><p>
  
  * <code><a href="https://cert-manager.io/docs/">Cert manager</a></code>
  
  * <code><a href="https://cert-manager.io/docs/concepts/ca-injector/">Cert manager Cainjector</a></code>
  
  * <code><a href="https://bryanbende.com/development/2021/07/01/k3s-raspberry-pi-cert-manager">Cert-manager in K3s</a></code>
  
  * <code><a href="https://www.techtarget.com/searchitoperations/tip/Learn-to-use-Kubernetes-CRDs-in-this-tutorial-example">CRDs </a></code>

  * <code><a href="https://www.howtogeek.com/devops/what-are-kubernetes-custom-resource-definitions-crds/">K8s CRDs </a> </code>
  
</p></details> 


<details><summary>Certificates</summary><p>
  
  * <code><a href="https://ikarus.sg/why-traefik-ingress-controller/">Kubernetes Ingress Controllers</a></code>
  
  * <code><a href="https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/">Ingress Controllers</a></code>
  
  * <code><a href="https://traefik.io/glossary/kubernetes-ingress-and-ingress-controller-101/">What is a Kubernetes Ingress Controller?</a></code>
  
</p></details> 

