## Implementation of OpenID Connect to improve security of kube-apiserver

The kube-apiserver is the forefront of kubernetes architecture and plays the most crucial role. Since with role comes responsibility, hence whenever any user tries to access the cluster, this core component authenticates them based on client-certificates, ensuring that any incoming request is TLS protected. It is to be noted that these client-certificates used by kube-apiserver to authenticate the users have an extreme limitation that they are very long-lived i.e. have no expiry date and cannot be revoked effectively. This implies, in case of any cluster attack happening, the admin has almost negligible opportunity to survice it thsu leading the cluster to crash with no applications and services running. Furthermore, these certificates cannot support any user an group management for human users thus enabling any user to access the kubernetes resources. A kube-apiserver when compromised or under attack can be evident to various malicious pods being exceuted in the cluster leading to attacks like cryptojacking. This attack allows a hijacker to gain the access and further steal the computing resources like CPU, Memory etc. which are needed to run the containers in our clusters in order to mine cryptocurrencies for his personal agenda or benefit. This gives rise to extend the security of this very crucial component of the kubernetes architecture using the token-based authentication protocol called OpenID Connect (OIDC). It is built on top of an authorization server called OAuth 2.0 and adds the missing authentication layer to the authorization protocol. OIDC uses the same components and architecture as OAuth, but to authenticate. It does so with the help of the actors that OIDC has in its architecture. They are:

```

1. OpenID Provider (OP): 

   - It is the OAuth2.0 authorization server.

2. Relying Party (RP):

   - IT is the application which is the requesting the end-user authentication and information.

3. End-user:

   - The human participant that is authenticated and about whom the relying party is requesting information
```

In this demonstration, we will implement OIDC using Keycloak as the OpenID Identity Provider which is further connected to our kube-apiserver followed by setting up an RBAC rule to allow permissions to specific users categorized under the namespace to perform the requested actions.


### Setup Keycloak:
