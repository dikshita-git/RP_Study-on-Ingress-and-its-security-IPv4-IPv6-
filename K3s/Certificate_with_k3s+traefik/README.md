# Certificate for k3s Ingress with Traefik

In this particular folder, our primary focus is on using default Wildcard certificate for K3s's in-built Ingress Controller ***Traefik***. To deep dive into the steps followed and their declarative aspects, here is a basic draft about <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Traefik">Traefik</a> to read.

Below marks the steps followed for the tassk with their description:

## Steps:


### 1. Set up the k3s Cluster

Here is my k3s cluster set-up <code> <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Cluster-setup">file</a></code> .


### 2. Install cert-manager (using helm)

Once our cluster is up and running, we can now install Cert-manager. Read more about <code> <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/TLS-in-Kubernetes#cert-manager">Cert-manager</a></code>

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Btraefik/helm_install.PNG" height="300">
<p><i>Fig: Installing cert-manager using helm</i></p>



Now we could check ns to see if cert-manager is running:

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Btraefik/cert-man_ns.PNG">
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

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Btraefik/cert-mang-cainjector-solution.PNG" height="300">
<p><i>Fig: Final cert-manager Cainjector pod</i></p>


### 3. Create and apply ClusterIssuer

Here is my <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Certificate_with_k3s%2Btraefik/cert-manager/ClusterIssuer.yaml">ClusterIssuer</a></code> file

Issuers, and ClusterIssuers are Kubernetes resources that represent certificate authorities (CAs) that are able to generate signed certificates by honoring certificate signing requests. All cert-manager certificates require a referenced issuer that is in a ready condition to attempt to honor the request. <a href="https://cert-manager.io/docs/concepts/issuer/">Read More</a>

I am using the ACME issuer type with DNS01 challenges via Cloudflare. This involves me first needing to get an API token from Cloudflare and then providing it to K3s as a Secret resource as <code>api-token</code> of the ClusterIssuer file.

The benefit of using a ClusterIssuer (over a standard Issuer) will make it possible to create the wildcard certificate in the kube-system namespace that K3s uses for Traefik. Also, note that any referenced Secret resources will (by default) need to be in the cert-manager namespace.


### 4. Requesting a wildcard Certificate

This <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Certificate_with_k3s%2Btraefik/cert-manager/Certificate.yaml">Cerficate.yaml</a></code> file requests for a <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/tree/main/K3s/Research-answers/2.%20Certificates">Wildcard certificate</a></code>for my domain <a href="https://dkrp2.xyz/">dkrp2.xyz</a>.

If we check the ***kubectl describe certificate -n kube-system***,  we will see the message "Certificate issued successfully." 


### 5. Configure traefik

The final step includes configuring the default ingress controller in k3s : Traefik so that the domain uses the wildcard we requested for in the above step. This step is needed because if the Ingress resource is being defined without any TLS secretname then Traefik chooses the default TLS certificate as it has no input about the new/custom certificate we received. traefik.yml. Thus we have to mount our custom wildcard certficate in such a manner that Traefik choose it instead of opting for  its default TLS certificate. 

In order to do that, we have to add the beow to the vauesContent part in /var/lib/rancher/k3s/server/manifest/traefik.yml file as shown in the image below:

```
extraVolumeMounts:
  - name: ssl
    mountPath: /ssl
extraVolumes:
  - name: ssl
    secret:
      secretName: wildcard-dkrp2-tls     
```

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Btraefik/tarefik.yaml.PNG" height="300">
<p><i>Fig: My traefik file after the content</i></p>


This <code>secretName</code> should be the same as mentioned in the <code><a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/K3s/Certificate_with_k3s%2Btraefik/cert-manager/Certificate.yaml">Certificate.yaml</a></code> file.


***What does adding these contents imply?***

<code>extraVolumeMounts</code> means basically adding an extra storage and allows us to mount or attach multiple type of volumes in a pod. In case our cert-manager pod's container data gets deleted or lost or even if the container restartes or crashes, the newly created pod's container can immediately grab this data at the state before the container crashes. In this case it will use the path /ssl to mount the volume or (certificate in this case) on. Here, <code>extraVolumes</code> contains the actual data of our own wildcard certificate with the secretname <code>wildcard-dkrp2-tls </code> that is mentioned in certificate.yaml and this means that the generated wilcard certificate is to be attached and used instead of defaul TLS certificate.

Finally, we have our certificate attached to the domain:

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Certificate_with_k3s%2Btraefik/result.PNG" height="300">
<p><i>Fig: TLS on domain</i></p>

