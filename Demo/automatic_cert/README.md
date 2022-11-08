## Auto-generate and auto-renew certificate for k3s ingress with Traefik


As automatic deployments of certificates and their auto-renewal plays a pivotal role in easing the development, hence here is a demonstartion of teh same with <a href="https://dkrp2.xyz/">my domain</a> where I have used cert-manager and integrated it with Lets-Encrypt.

Below marks the steps followed for the task with their description:

## Steps:


## 1. Set up the k3s Cluster:

1.1) Install K3s server in any node we wish to make it as control plane or a master using the command below:

```
curl -sfL https://get.k3s.io | sh -
```

1.2) Join the agent or worker nodes to this server node

This is achieved by copying the node-token under <code>/var/lib/rancher/k3s/server/node-token</code> and putting it in 

```
curl -sfL https://get.k3s.io | K3S_URL=https://[IP_address_of_master_node]:6443 K3S_TOKEN="PASTE_TOKEN" sh -
```

## 2. Install cert-manager (using helm):

Once our cluster is up and running, we can now install Cert-manager. Read more about <code> <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Wiki#22-tls-and-its-working-principle">Cert-manager</a></code>

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/manual_cert/Certificate_with_k3s%2Btraefik/helm_install.PNG" height="300">
<p><i>Fig: Installing cert-manager using helm</i></p>


Now we could check ns to see if cert-manager is running:

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/manual_cert/Certificate_with_k3s%2Btraefik/cert-man_ns.PNG">
<p><i>Fig: Checking namespace named cert-manager</i></p>


>***Note***
>There is a possibility that <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/TLS-in-Kubernetes#cainjector-in-cert-manager">cert-manager cainjector</code></a> shows the status of "Crashloopbackoff".
>  - In such cases, to get more details, check the log file of cert-manager ca-injector by:

```
kubectl logs -n cert-manager <cert-manager-cainjector-name-found-from-get-pods --namespace cert-manager>
```
>   - Next install the higher version of cert-manager yaml by :
     
```
k3s kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.4.0/cert-manager.yaml
```      

Here I tried to apply a higher version of cert-manager. v1.2.0 already moved to use "admissionregistration.k8s.io/v1".


## 3. Deploy the application:

A namespace called "dev" is created using the command:

```
k3s kubectl create ns dev
```  

<a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/automatic_cert/ingressroute.yaml">IngressRoute</a> is a resource file consisting of its name "whoami" and entrypoints. Whoami will listen to all the incoming requests to <a href="https://dkrp2.xyz/">dkrp2.xyz</a> on the websecure entrypoint. Traefik handles HTTPS requests using entrypoint "websecure" | port 443. Traeik has the role to perform HTTPS exchange and once established it will delegate the requests to my <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/automatic_cert/service.yaml">service</a></code> named as "whoami".


## 4. Generation of automatic certificate for the application:

We used Lets Encyrpt for this case and it will generate and renew the certificates itself. This is accomplished by creating a <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/automatic_cert/traefik-config.yaml">traefik-config file</a></code> and adding <code>certificatesresolvers</code> to it which holds the details of Lets Encrypt account, challenges, servers and certificate storage. In order to avoid the Lets Encrypt rate limit, I have used acme-staging server for all non-production environments.


## 5. Re-configure ingressroute file 

Here we need to add <code>tls.certResolver</code> to <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/automatic_cert/ingressroute.yaml">ingressroute file</a></code> which will associate with the <code>certificatesresolvers</code>.


## 6. Set TLS option:

Traefik has an advantage that it provides multiple options to control and to configure the various aspects of TLS handshake like TLS versions, exchange ciphers etc. which might be a weak category in the deployed application. This can be checked in <a href="https://www.ssllabs.com/">SSLabs</a>. While I made a check for my domain, I got the results as grade B.

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/dkrp2_before_TLS.png">
Fig: TLS scenario before 


## 7. Summary

Applying the steps above, now the web application <a href="https://dkrp2.xyz/"></a> holds a Lets Encrypt certificate as shown in the figure below:

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/lets_encyrpt_domain.png">
Fig: Lets encrypt certificate


