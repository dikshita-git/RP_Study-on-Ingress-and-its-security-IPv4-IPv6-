## Authenticating kubernetes based on service token and Role-based access control

A special thanks to <a href="https://hub.docker.com/r/bibinwilson/docker-kubectl">Bibin Wilson</a> for the reference to kubectl utility image used in this experiment.

Allowing limited access to kubernetes resources are a foundation ot the security of the cluster thus protecting it from malwares and phishing attacks. By default, kubernetes uses client certificates, bearer tokensor authentication proxy in order to authenticate the incoming API requests through the authentication plugins. Authentication in kubernetes validation of ***"who"*** and ***"what"*** is issuing the requests. Parallel to it, authorization means ***"verifies whether a certain action to list pods or services is allowed or permitted to a certain user(under a certain namespace may be)."***

Usually the API server authenticates every incoming human-made request (not a program/pod request). By default kubernetes does not maintain any database to store the credentials of the users. This means that it expects it to be managed by an external source. Thus, the human user accounts have to be managed by one of the supported authentication strategies like:


### 1.  <ins>X509 certificates:</ins>

* Simplest method to authenticate.

* We must first create a private key and a Certificate Signing Request, or CSR, for the user account you want to authenticate for using this authentication strategy.

* ***Disadvantage:***
 
Though it is a secur way to access the kubernezes cluster, but it has a biggest disadvantage that the certificates isssued by the cluster cannot be revoked. In other words, if this issued certificate gets stolen, it will still remain active until it expires by its expiry date or unless the adminsitrator replaces the entire CA in the cluster.

* ***Suitable solution to reduce risk:***

Issuing certificate with shorter validation dates.



### <u>2. Static token file / Static passsword file:</u>

* It is a type of method in which the administrator defines a set of credentials for users in a <code>password.csv</code> file without defining or configuring any validity period.

* This <code>password.csv</code> consists of password, username and userID, respectively, for the users we want to authenticate.

```
password1,user_1,user1_ID

password2,user_2,user2_ID

```

### <u>3. Bootstrap Tokens:</u>

```

```

### <u>4. Service account tokens:</u>

```

```

