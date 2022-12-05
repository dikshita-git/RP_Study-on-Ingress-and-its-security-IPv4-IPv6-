## Authenticating kubernetes based on service token and Role-based access control

A special thanks to <a href="https://hub.docker.com/r/bibinwilson/docker-kubectl">Bibin Wilson</a> for the reference to kubectl utility image used in this experiment.

Allowing limited access to kubernetes resources are a foundation ot the security of the cluster thus protecting it from malwares and phishing attacks. By default, kubernetes uses client certificates, bearer tokensor authentication proxy in order to authenticate the incoming API requests through the authentication plugins. Authentication in kubernetes validation of ***"who"*** and ***"what"*** is issuing the requests. Parallel to it, authorization means ***"verifies whether a certain action to list pods or services is allowed or permitted to a certain user(under a certain namespace may be)."***

Usually the API server authenticates every incoming human-made request (not a program/pod request). By default kubernetes does not maintain any database to store the credentials of the users. This means that it expects it to be managed by an external source. Thus, the human user accounts have to be managed by one of the supported authentication strategies like:


### 1.  <ins>X509 certificates:</ins>

* Simplest method to authenticate.

* We must first create a private key and a Certificate Signing Request, or CSR, for the user account you want to authenticate for using this authentication strategy.

* 🟥 ***Disadvantage:***
 
Though it is a secure way to access the kubernezes cluster, but it has a biggest disadvantage that the certificates isssued by the cluster cannot be ***revoked***. In other words, if this issued certificate gets stolen, it will still remain active until it expires by its expiry date or unless the adminsitrator replaces the entire CA in the cluster.

* 🟩 ***Suitable solution to reduce risk:***

Issuing certificate with shorter validation dates.



### <ins>2. Static token file / Static passsword file:</ins>

* It is a type of method in which the administrator defines a set of credentials for users in a <code>password.csv</code> file without defining or configuring any validity period. Best for beginner in kubernetes

* This <code>password.csv</code> consists of password, username and userID,followed by optional group names respectively, for the users we want to authenticate.

```
password1,user_1,user1_ID,"group1,group2,group3"

password2,user_2,user2_ID,"group1,group2,group3"
```

* Once this file is created, simply identify it on the kube-apiserver command line using the flag <code>--basic-auth-file=filename</code>.

* 🟥 ***Disadvantage:***

i) Since it is a static file, so any person who gains access to it can modify the file.
ii) Additionally, this file should be manually modified every time for any changes in its data.



### <ins>3. Bootstrap Tokens:</ins>

* These are 23-character bearer tokens which are stored as secrets in the kubernetes cluster which are then issued to individual kubelets and are meant to be used when creating the cluster or while joining nodes in the cluster.

* These Secrets are then read by the Bootstrap Authenticator in the API Server. 

* Expired tokens are removed with the TokenCleaner controller in the Controller Manager. 

* Consists two distinct parts separated by a dot: <code>{token-id}</code>.<code>{token-secret}</code>


```
1. <code>{token-id}</code>, with the size of six characters, is considered the public information of the key that identifies the token and used when referring to a token without leaking the secret part used for authentication. . 

2. <code>{token-secret}</code> acts as the secret and should only be shared with trusted parties..
```

* 🟥 ***Disadvantage:***

No expiration dates.


### <ins>4. Service account tokens:</ins>

* These are the most fundamental tool for the pods to access the kube API server.

* It is an automatically enables authenticator by the kube API server  that uses signed bearer tokens to verify requests and are associated with pods running in the cluster through the ServiceAccount Admission Controller.

* Service account bearer tokens are perfectly valid to use outside the cluster and can be used to create identities for long standing jobs that wish to talk to the Kubernetes API.


### STEPS:

#### 1. Verify if RBAC is enabled in the kubernetes version :

Before proceeding, it is recommended to check the RBAC if enabled in order to get the version number as it might change as per kubernetes version.

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/images/check_rbac_enabled_new.png">
Fig: Verify the RBAC 


#### 2. Create serviceaccount:

For this experiment, a read-only serviceaccount named "sa" is created which has access to only the "default" namespace of the kubernetes cluster. This is done using the command:

```
kubectl create serviceaccount sa -n default
```

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/images/2.png">
Fig: Create serviceaccount

