
1. add oidc configuration to kube-apiserver
source: 
	https://developer.okta.com/blog/2021/11/08/k8s-api-server-oidc
	https://kubernetes.io/docs/reference/access-authn-authz/authentication/

a) first we choose which claims to use for mapping username and groups to it
we have these values in our claims:
  "groups": [
    "kubernetes_role",
    "manage-account",
    "manage-account-links",
    "view-profile"
  ],

  "preferred_username": "testuser",

https://kubernetes.io/docs/reference/access-authn-authz/authentication/
to map groups we use
	--oidc-groups-claim	
		JWT claim to use as the user's group. If the claim is present it must be an array of strings.
	-> we will set it to "groups", which is our new claim

to map username we use
	--oidc-username-claim
		JWT claim to use as the user name. By default sub, which is expected to be a unique identifier of the end user. Admins can choose other claims, such as email or name, depending on their provider. However, claims other than email will be prefixed with the issuer URL to prevent naming clashes with other plugins.

	we have
  		"preferred_username": "testuser",
		"sub": "ed6353e9-b1eb-4d7f-9aee-eec5e6fa7cce",
	-> we will use preferred_username here, but sub or email would be possible aswell
	-> no human can identify the sub by this name
	-> so preferred_username is the better choice to be used as username claim
    -> sub is a uuid for each user

b) apply configuration and extend arguments of kubeapiserver

sudo vi /etc/kubernetes/manifests/kube-apiserver.yaml
    - --oidc-issuer-url=https://10.20.116.209.nip.io/realms/master
    - --oidc-client-id=kubernetes
    - --oidc-username-claim=email
    - --oidc-groups-claim=groups
-> we do not need a secret here, since this endpoint is only used to verify the JWT's signature
-> kube-apiserver only talks to the webfinger url https://10.20.116.209.nip.io/realms/master/.well-known/openid-configuration
-> and there finds out the keys at
	https://10.20.116.209.nip.io/realms/master/protocol/openid-connect/certs

kubectl apply -f /etc/kubernetes/manifests/kube-apiserver.yaml

c) trust the CA
currently the kube-apiserver can't verify the authenticy of the self-signed certificate
	k logs -n kube-system kube-apiserver-master -f
		E0119 03:32:53.663085       1 oidc.go:335] oidc authenticator: initializing plugin: Get "https://10.20.116.209.nip.io/realms/master/.well-known/openid-configuration": x509: certificate signed by unknown authority

the ca-certificates are mounted into the kube-apiserver container (see /etc/kubernetes/manifests/kube-apiserver.yaml)
  volumes:
  - hostPath:
      path: /etc/ssl/certs
      type: DirectoryOrCreate
    name: ca-certs
  - hostPath:
      path: /etc/ca-certificates
      type: DirectoryOrCreate
    name: etc-ca-certificates
  - hostPath:
      path: /etc/kubernetes/pki
      type: DirectoryOrCreate
    name: k8s-certs
  - hostPath:
      path: /usr/share/ca-certificates
      type: DirectoryOrCreate
    name: usr-share-ca-certificates



we also want to do this on our laptop
obtain our ca certificate like so:
scp master2@10.20.116.209:~/myssl/root-ca.pem .
(ca certificate needs to be in master2 home directory)

to add trust for the own CA certificate it is added to /etc/pki/ca-trust/source
trust it on local machine and on node
trust on both nodes although we only need it on master, because there is kube-apiserver
normal os:
	$ sudo trust anchor --store root-ca.pem
ubuntu:
    $ sudo apt-get install -y ca-certificates
    $ sudo cp root-ca.pem /usr/local/share/ca-certificates/root-ca.pem # ENDING IMPORTANT
    $ sudo update-ca-certificates

copy certificate from master2 to worker2
    scp ~/myssl/root-ca.pem worker2@10.20.116.231:~

btw: the certificate inside keycloak with which it signs the tokens is not equal to the certificate of the https:// that means TLS based website
The JWT tokens is trusted via the Issuer URL
    This Issuer URL is appended by .well-known/openid-configuration (this is webfinger, which has its own specification)
And the TLS of the keycloak website, which includes token endpoint and authorization endpoint and userid endpoint is trusted by what we do right now "trust anchor ..."
    

The certificate will be written to /etc/ca-certificates/trust-source/myCA.p11-kit and the "legacy" directories automatically updated. 
the target foler is already mounted into kube-apiserver

if it doesn't work right away, we can kill the kube-apiserver container and restart kubelet:
    sudo crictl ps
    sudo crictl stop a225ebb26250e <- containerid
    sudo systemctl restart kubelet

k logs -n kube-system kube-apiserver-master -f
	-> there should be no errors


