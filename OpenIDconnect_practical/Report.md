## Title

Granular access control to kube-apiserver using OpenID Connect 


## Topic

Kube-apiserver component of the kubernetes architecture is the core of the control plane. Once it is compromised, several malicious pods can be started leading to hijacking of our cluster resources. This can result in applications and services being unavailable ultimately leading to no request handling. Thus, the goal of this project is to find a way to improve the security of this component.

## Team

***Supervisor:*** Prof. Dr Martin Leischner

***Mentor:*** Richard Clauß

***Candidate:*** Dikshita Kalita (Matriculation: 11137758)


## Table of Contents:

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#1-motivation">1. Motivation</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#2-research-question">2. Research Question</a>

* <a href="">3. Fundamentals</a>

   * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#341-basics-of-oauth-20-open-authorization">3.1 OAuth 2.0</a>
        
      * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#3411-key-terminologies-in-oauth-20">3.1.1 Roles and key terms</a>

      * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#3412-oauth-endpoints">3.1.2 Endpoints</a>
    
      * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#3413-authorization-flows">3.1.3 Authorization Flows</a>

      * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#3414-pitfall-of-oauth-20">3.1.4 Pitfall of OAuth 2.0</a>

   * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#342-emergence-of-openid-connect"> 3.2 Emergence of OpenID Connect</a>
     
      * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#3421-key-concepts">3.2.1 Roles and key terms</a>

      * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#3422-endpoints">3.2.2 Endpoints</a>

      * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#3423-authentication-flows">3.2.3 Authentication Flows</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#3-realizing-a-solution">4. Realizing a solution</a>

     * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#31-kinds-of-attacks-on-kube-apiserver">4.1 Kinds of attacks on kube-apiserver</a>

     * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#32-causes-of-attacks">4.2 Causes of attacks</a>

     * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#33-default-kubernetes-authentication-strategies">4.3 Default kubernetes authentication strategies and their pitfalls</a>

     * <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#4-how-openid-connect-can-improve-the-security-of-kube-apiserver">4.4 How OpenID Connect can improve the security of kube-apiserver? </a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#5-state-of-the-art">5. Adoption of OIDC</a>
  
* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#6-openid-connect-with-keycloak-in-practice">6. OpenID Connect with Keycloak in practice</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#6-results">7. Results</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#8-summary">8. Summary</a>

* <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#9-literature">9. Literature</a>




----------------------------------------------------


## 1. Motivation

This project aims to find an improved way in order to secure the kube-apiserver from an attack called "cryptojacking". It is an attack wherein the attacker gains access to the cluster and steals the computing resources like CPU, Memory etc. which are needed to run our containers. Once he succeeds in stealing them, he can use them further to mine cryptocurrencies like Monero. Although there are several possible attack surfaces but the primary, of course not exclusive component is the kube-apiserver.

--------------------------------------------------


## 2. Research Question

How can the security of the kube-apiserver be improved against compromised client certificates?

---------------------------------------



## 3. Realizing a solution


### 3.1 Kinds of attacks on kube-apiserver

Besides cryptojacking, various types of attacks are viable through attacking kube-apiserver. Some of these includes:

i) Using kube-apiserver as the attack surface can help an attacker to spread his malwares in the clusters leading to unavailability of the hosted applications and services.

ii) Attacker could take up the advantage of the API server communication into communicating with some malicious resources which are hosted in the externals.


### 3.2 Causes of attacks

Whenever any operator tries to access the cluster using kubectl, this command line interface uses a client certificate and client key to get authenticated by the kube-apiserver and sends an HTTPS request. This set of client certificate and key is primarily generated when we create a cluster and is stored in the kubeconfig file which is used to store the cluster authentication informations for kubectl. In order to attack the kube-apiserver, an attacker needs to obtain this client-certificate because it contains the signature of the kubernetes certificate authority and the client-key in order to prove that the he is the owner of the certificate.
Another prospects for an attacker to be able to steal this certificates are:

- These client-certificates are infinitely long-lasting and 
- They cannot be revoked effectively. 

This is due to the fact that kubernetes is not able to check the status of any provided client-certificate. Also, it does not implement either [:bulb: "Certificate Revocation List" (CRL)](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#certificate-revocation-list-crl) or [:bulb: "Online Certificate Status Protocol" (OCSP)](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#online-certificate-status-protocol-ocsp).

##### :bulb: Certificate Revocation list (CRL): 

 - It is a list consisting of all the digitally signed certificates which are revoked by the certificate authority before their scheduled date of expiration and also that it should not be trusted anymore.

##### :bulb: Online Certificate Status Protocol (OCSP): 

 - It is an internet protocol enabling an application to determine the revocation status of the identified certificates without the usage of CRL. 



### 3.3 Default kubernetes authentication strategies

Kubernetes by default provides some authentication strategies like:

#### *i) Static-token:*

 - Here, the kubernetes admin will create a comma-seperated value file (CSV) wherein he will list down password / token, username, UID, group(s)(optional) in the format as shown below:

```
<token>,<username>,<UID>,”<group1, group2>”
<token>,<username>,<UID>,”<group2, group4>”
```

:small_red_triangle_down: **Disadvantage**:

- This CSV file is not at all scalable.

This infers that every time with the increased or decreased number of users, this file has to be modified and kube-apiserver has to be restarted after every modification for the changes to be reflected. This restart might lead to kube-apiserver being crashed.

#### *ii) X509 client-certificates:*

In this certificate-authentication method, we  need to:

- Create a certificate request, 
- Sign it through a Certificate Authority (CA) and presents it to the API server during the authentication phase. 
- The API server the consults the CA server to validate the certificate and approves or denies the request accordingly.

To create a certificate signing request and key for user (her e I created for myself) using the command:

```
openssl req -new -newkey rsa:4096 -nodes -keyout dikshita-k8s.key -out dikshita-k8s.csr -subj "/CN=dikshita/O=dev-student"
```
:bulb: Various tools like easyrsa, openssl etc. can be used to generate the certificate request.

Once the CSR and key is generated, we can retrieve the encoded csr using:

```
sudo cat dikshita-k8s.csr | base64 | tr -d '\n'
```

Next, we should create the CSR for kubernetes. The encoded csr received should now be included in a YAML file as shown below:
![](https://codimd.s3.shivering-isles.com/demo/uploads/78ab250e-47d7-4388-99d4-bb176ba887da.png)
<p style="text-align: center;">Fig: Including encoded CSR into YAML file</p>

Once this CSR is created by kubernetes, we have to approve it which is followed by creating a CA certificate for user:dikshita because CLI kubectl client of user:dikshita needs to trust the server, so that the cluster can authenticate the user. Now, we can create a kubeconfig file for user:dikshita where we will store these received certificates.

This method could be a good authentication strategy but carries some pifalls with it:

:small_red_triangle_down: **Disadvantage:**

- Very unsuitable for production uses because kubernetes does not support any kind of revocation of certificates.

As we have seen that though kubernetes supports some authentication strategies by default, they have some considerable downfalls which can be summarized as:

- Non-scalable
- Not for bigger production purposes
- Not enough secured to fight against the strength of the attacks because kubernetes does not provide any kind of human user-management and rather expects it to be managed externally.

This is where OpenID Connect comes into the frame.


### 3.4 Token-based authentication with OpenID Connect:

To get started with OpenID Connect, we need to first learn a bit about [:bulb: OAuth 2.0](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#small_blue_diamond-oauth-20-open-authorization) because OIDC is built on top of it.


#### 3.4.1 Basics of OAuth 2.0 (Open Authorization): 

- It is a standard designed to grant permission to a website or application to access the resources hosted by other applications on behalf of the user. 

- Use-case of OAuth 2.0:

![usecase-oauth](https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/oauth_usecase.png)
<p align=center>Fig: Scenario of OAuth 2.0 usage, Source: <a href="https://developers.google.com/identity/protocols/oauth2/limited-input-device">By GoogleDevelopers</a></p>

- In the above figure, an user tried to create an account in an app called example and chooses to Sign in with Google.
- Once he selects it, he is redirected to a page provided by Google asking his consent if he allows this application o view his basic profile an view all his email addresses.
- This happens because the application trusts Google as the identity provider in this case and if the user already has an account in the identity provider and if he gives his consent then he can be successfully signed in to the application he wants to register with.
- This process explains why OAuth 2.0 is said to be used for delegated authority.

#### 3.4.1.1 Roles and key terms in OAuth 2.0:

There are several terminologies in OAuth which forms a fundamental block of understanding its actions as described in the table below:

 
|                             |    OAuth Terms | Description | 
|-----------------------------| ------------| ----------- |
|<img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/image.png" width="100"/> | **Resource Owner** | We! Human users because in this context we are the owner of our own identity, our data and any actions that can be performed with our accounts.  |
| <img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/client.png" width="62" />  | **Client** |  - It is the application that wants to access data on behalf of the resource owner. <p>- Eg: Example application as per the figure.</p> |
| <img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/authZ_server.png" width="862" />        |   **Authorization Server**    |  - This is where the resource owner has his account on.<p>- Eg: Google, Github, LinedIn, Keycloak</p>  |
| <img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/resource_server.png" width="862" />           |**Resource Server**     | - It is basically the OAuth term for API server. <p>- It can accept or respond to the protected resource requests using the access tokens</p>   |
|  <img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/redirect_uri.png" width="210" />                  | **Redirect URI**  |  - It is the "Callback URL". <p>- Authorization Server will redirect the Resource Owner back to this Callback URL after granting permissions to client.</p>  |
|  <img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/code.png" width="100" />             |   **Response Type**  | It is the type of information which the client expects to receive. <p>- One of the most common type of Response type is ***code*** in which the client expects an ***Authorization Code***. </p>  |
| <img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/scope.jpg" width="60" />   |   **Scope**  |   These are fine granular permissions which the client wants. Eg: accessing data, performing actions etc.   |
|  <img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/consent.png" width="300" />  <p align=center> Source: <a href="https://developers.google.com/identity/protocols/oauth2/limited-input-device">By GoogleDevelopers</a></p>           |  **Consent**     |  Authorization Server takes the scopes which the client is requesting, and then verifies with the Resource Owner whether or not they want to give the client the permissions.  |
|  <img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/client_id.png" width="150" />  |  **Client ID**   |    Client is identified with the Authorization server using this ID.   |
|  <img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/client_secret.png" width="150" /> |  **Client Secret**| This is a secret password that only Client and Authorization server knows and uses it to securely share informations privately. |
| <img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/auth_code.png" width="40" />  | **Authorization Code** |It is a short-lived temporary code which the client gives the Authorization Server in exchange for an Access Token. |
| <img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/access_token.png" width="140" />           |  **Access Token**    |   This is the key which the client uses to talk to Resource server . It gives client the permissions to request data or perform actions with the Resource server on our behalf. |

#### 3.4.1.2 OAuth Endpoints:

- Endpoints are URLs that provide OAuth clients the ability to call in order to request OAuth tokens or Authorization code.
- 3 most common endpoints are:

|  Endpoint name      |   Description             | 
|---------------------|---------------------------|
|   **1. Authorization Endpoint** ({your_domain}/oauth/v2/authorize)  |  <ul><li> It is used to interact with the resource owner (end-user) and get the authorization to access the protected resource.</li></ul>  |
|   **2. Token Endpoint** ({your_domain}/oauth/v2/token)    |  <ul><li> Used by the application in order to get an access token or a refresh token.</li><li> It is used by all flows except for the Implicit Flow because in that case an access token is issued directly.</li></ul> |
    


#### 3.4.1.3 Authorization Flows:

OAuth defines some flows in order to get the access token. These flows are called ***"Grant Types"***. These are listed in the table below:


|  Type of OAuth Flow                    |     Description     | Use-case     |             Workflow              |
|----------------------------------------|---------------------|----------------|---------------------------------|
| **1. Authorization Code Flow**         |  Exchanges an authorization code for a token  | Server side web applications where the source code is not exposed publicly |  <ul><li>The user clicks on a login link in the web application.</li><li>The user is redirected to an OAuth authorization server, after which an OAuth login prompt is issued.</li><li>The user provides credentials according to the enabled login options.</li><li>Typically, the user is shown a list of permissions that will be granted to the web application by logging in and granting consent.</li><li>The user is redirected to the application, with the authorization server providing a one-time authorization code.</li><li>The app receives the user’s authorization code and forwards it along with the Client ID and Client Secret, to the OAuth authorization server.</li><li>The authorization server generates an ID Token, Access Token, and an optional Refresh Token, before providing them them to the app.</li><li>The web application can then use the Access Token to gain access to the target API with the user’s credentials.</li></ul> |
| **2. Client Credentials Flow**  | Allows applications to pass their Client Secret and Client ID to an authorization server, which authenticates the user, and returns a token. This happens without any user intervention | M2M apps (daemons, back-end services and CLIs). <p>- In these types of apps, the system authenticates and grants permission behind the scenes without involving the user, because the “user” is often a machine or service role. It doesn’t make sense to show a login prompt or use social logins.</p> | <ul><li>The application authenticates with the OAuth authorization server, passing the Client Secret and Client ID.</li><li>The authorization server checks the Client Secret and Client ID and returns an Access Token to the application.</li><li>The Access Token allows the application to access the target API with the required user account.</li></ul>|
| **3. Resource Owner Password Flow**    |  Asks users to submit their credentials via a form.    |   Highly-trusted applications, where other flows based on redirects cannot be used.   | <ul><li>The user clicks a login link in the application and enters credentials into a form managed by the app.</li><li>The application stores the credentials, and passes them to the OAuth authorization server.</li><li>The authorization server validates credentials and returns the Access Token (and an optional Refresh Token).</li><li>The app can now access the target API with the user’s credentials.</li></ul> |
|**4. Implicit Flow with Form Post**     |  Uses OIDC to implement a web sign-in that functions like WS-Federation and SAML. The web app requests and receives tokens via the front channel, without requiring extra backend calls or secrets. With this process, you don’t have to use, maintain, obtain or safeguard secrets in your app.  |  Apps that don’t want to maintain secrets locally.  |  |
|  **5. Hybrid Flow**    |   Beneficial apps that can securely retain Client Secrets. It lets your app obtain immediate access to an ID token, while enabling ongoing retrieval of additional access and refresh tokens. This is useful for apps that need to immediately gain access to data about the user, but must perform some processing prior to gaining access to protected resources for a long time. |   Apps that need immediate access to data about the user, but also need to use this data on an ongoing basis.   |    |
|  **6. Device Authorization Flow**      |   This flow makes it possible to authenticate users without asking for their credentials. This provides a better user experience for mobile devices, where it may be more difficult to type credentials. Applications on these devices can transfer their Client ID to the Device Authorization Flow to start the authorization process and obtain a token.|   Apps running on input-constrained devices that are online, enabling seamless authentication via credentials stored on the device.    |      |
|  **7. Authorization Code Flow with PKCE**  |    This flow uses a proof key for code exchange (PKCE). A secret known as a Code Verifier is provided by the calling application, which may be verified by the authorization server using a Proof Key.    |   Apps that need to serve unknown public clients who may introduce additional security issues that are not addressed by the Auth Code Flow.   |       |

#### 3.4.1.3.1 OAuth 2.0 Authorization Flow:

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/OAauth_1.png" width="550" align="center"/> 
<img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/OAuth_2.png" width="550" align="center"/> 
<img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/OAuth_3.png" width="550" align="center"/> 
<p align=center>Fig: OAuth 2.0 Authorization Code Flow</p>

1. We, the Resource Owner, want to allow “Example app,” the Client, to access our contacts so they can send invitations to all our friends.
2. The Client redirects your browser to the Authorization Server and includes with the request the Client ID, Redirect URI, Response Type, and one or more Scopes it needs.
3. The Authorization Server verifies who we are, and if necessary prompts for a login.
4. The Authorization Server presents us with a Consent form based on the Scopes requested by the Client. We grant (or deny) permissions.
5. The Authorization Server redirects back to Client using the Redirect URI along with an Authorization Code.
6. The Client contacts the Authorization Server directly (does not use the Resource Owner’s browser) and securely sends its Client ID, Client Secret, and the Authorization Code.
7. The Authorization Server verifies the data and responds with an Access Token.
8. The Client can now use the Access Token to send requests to the Resource Server for your contacts.

Sources: [[1]](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#1-an-illustrated-guide-to-oauth-and-openid-connect-okta-developer-online-available-httpsdeveloperoktacomblog20191021illustrated-guide-to-oauth-and-oidc-accessed-jan-23-2023), [[2]](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#2-a-mizrachi-demystifying-oauth-flows-frontegg-oct-28-2021-online-available-httpsfronteggcomblogoauth-flows-accessed-jan-23-2023)



#### 3.4.1.4 Pitfall of OAuth 2.0:

OAuth 2.0 is exclusively used for authorization using an access token. But it has several pitfalls like:

- The design of OAuth specifications is vague.
- There are a few mandatory components that are needed for the rudimentary functionality of every grant type, but most of the implementation is entirely optional. This encompasses many configuration settings that are needed to keep users’ information secure.
- It lacks overall built.in security features.
- With this type of grant, highly sensitive data is also transferred via the browser, which provides many malicious opportunities for cybercriminals.

This is where OpenID Connect comes into picture.

#### 3.4.2 Emergence of OpenID Connect:

- It is a system entity that creates, maintains, and manages identity information for users and is an identity protocol that provides authentication services to relying applications.

>Since OAuth 2.0 ***does not*** provide any (standardized) way for the client to request or control user authentication, thus OIDC adds that missing layer of authentication.

- It provides authentication in the form of ID Tokens which encapsulates the identity of an user.

- Uses JSON as the data format.

- Enables us to externalize authentication.

- Leverages centralized authorization.


#### 3.4.2.1 Roles and key terms:

- :ballot_box_with_check: ***Actors:*** 
 The three actors that participate in the OpenID Connect authentication flow are listed in the able below:
 
| OpenID Provider (OP / IdP) | Relying Party (RP)| End-user|
| -------------------------- | ----------------- | ------- |
|  It is the OAuth2.0 authorization server that provides authentication as a service.     | - OAuth2.0 client application requesting end-user authentication and informations from Idp in order to authenticate users and request claims. <p>- **Eg**: Kubernetes</p>  | Human participant that is authenticated and about whom the relying party is requesting informations for. |


- :ballot_box_with_check: ***Terms used:*** 

| Terms | Description | 
| -------- | -------- | 
| 1. Claims   |- These are name / value pairs that consists of information about an user along with meta-data of OIDC service.  <p>- **Eg**: name, email, address, phone number etc. </p>   |
| 2. Scope   | - Collection of claims is called scope.  <p>- **Eg**: Profile scope contains: <ul><li>name,</li><li>email,</li> <li>address, </li><li>phone number </li></p>|
| 3. ID Token   | - It is a [JSON Web Token (JWT)](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#bulb-jwt):bulb: that encapsulates an user's identity. <p>- The main attributes that an ID Token has are: <ul><li>**Issuer**: authorization server that issued this token.</li><li>**Subject**: identifier for the end-user.</li><li>**Audience**: Relying Party or client who can use this token to authenticate.</li><li>**Expiration**: expiry time of the token. Can be in minutes, hours, days etc.</li</ul></p>|
| 4. Access Token   |  - These are credentials that are used to access protected resources. <p>- They do not carry any information about the user.</p>   |
| 5. Refresh Token   | - These are credentials issued by the authorization server that are used to obtain the access tokens. <p>- It enables us to have short-lived access token without having to collect the credentials once these access token expires.</p>    |
| 6. UserInfo Endpoint   |- This is the protected resource that returns value information about the end-user.     |


<p align=center>Fig: JWT credentials received from Identity Provider</p>

##### :bulb: JWT:

- Compact token format intended for HTTP authorization headers and URI query parameters.
- These tokens are base64 URL encoded and are digitally signed and encrypted using secert HMAC or RSA which ensures their protection from being modified by an attacker or any client.
- A JWT has three main parts:

   **i) Header:** This part consist of the algorithm name (Eg: HMAC SHA256, RSA SHA256 etc.) and type of token (Eg: JWT). It is a Base64Url encoded to form the first part of the JWT format.

  **ii) Payload:** It consists of claims like username, token issued date etc. These informations are very useful to know the details about the owner of token, date of token issue etc.  

  **iii) Verify signature:** The above header and payload is then digitally signed by the Identity provider using signature algorithm from the header. This signature verifies that the issuer (Identity Provider) agrees to his identity and assures that the payload / message was not changed all its way.


The parts are shown in the image below:

<kbd>![jwt_parts](https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/parts_jwt.png)</kbd>
<p align=center>Fig: Parts of JWT, Source: <a href="https://supertokens.com/blog/what-is-jwt">By SuperTokens Team</a></p>


#### 3.4.2.2 Endpoints:

OpenID Connect too has similar endpoints just like in OAuth 2.0.  The most commonly used one are:

|  Endpoint name      |   Description             | 
|---------------------|---------------------------|
|   **1. Authorization Endpoint** ({your_domain}/oidc/endpoint/keycloak/authorize)  |  <ul><li>  It is the starting point for all initial user authentications. The user agent (browser) will be redirected to this endpoint to authenticate the user in exchange for an authorization_code (authorization code flow) or tokens (implicit flow).</li></ul>  |
|   **2. Token Endpoint** ({your_domain}/oidc/endpoint/keycloak/token)    |  <ul><li> It return various tokens (access, id and refresh) depending on the used grant_type.</li><li>When using Authorization code flow call this endpoint after receiving the code from the authorization_endpoint.</li></ul> |
|  **3. UserInfo Endpoint**  ({your_domain}/oidc/endpoint/keycloak/userinfo     | <ul><li>This endpoint will return information about the authorized user.</li></ul>   |
| **4. Discovery endpoint** ({your_domain}/.well-known/openid-configuration)     | <ul><li> Located within the issuer domain.</li><li>THis endpoint provides a client with configuration details about the OpenID Connect Authorization Server</li></ul>    |
|  **5. JSON Web Key (JWK) endpoint** ({your_domain}/oidc/endpoint/keycloak/jwk)   | JSON Web Key Set (JWKS) endpoint is a read-only endpoint that returns the Identity Provider's public key set in the JWKS format.                  |
|  **6. Revocation endpoint** ({your_domain}/oidc/endpoint/keycloak/revoke)   | This endpoint enables clients to revoke an access_token or refresh_token they have been granted.    |

Sources: [[5]](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#5-endpoints--zitadel-docs-online-available-httpswwwzitadelcomdocs-accessed-jan-23-2023), [[6]](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#6-json-web-key-set-endpoint---identity-server-541---wso2-documentation-online-available-httpsdocswso2comdisplayis541jsonwebkeysetendpoint-accessed-jan-23-2023), [[7]](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#7-ibm-documentation-online-available-httpswwwibmcomdocsensva905topicssprek_905comibmisamdocconfigconceptoauthendpointshtm-accessed-jan-23-2023)


#### 3.4.2.3 Authentication Flows: 

OIDC supports 4 types of authentication flows:

|   Flow name                 |   Use-case           |     Description             |
|-----------------------------|-----------------------------|---------------------------|
| **1. Authentication (or Basic) Flow**    | - Used for apps that have a back end that can communicate with the Identity Provider away from prying eyes. <p>- This is the flow suitable for the project because our Client (kubectl) and User-Agent (browser) are seperate entities and we need a secured mod of code exchange.</p> |   Described [Here](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#34231-authentication-flow-in-detail)   |
| **2.  Implicit Flow**   |  Used in apps in which client and user-agent is the same entity. Eg: Apps in mobile phones | -  Here a public/private key (JSON Web Key or JWK) scheme is used to encrypt or sign user details. <p>- When you register your client app with the IdP (OneLogin), you will receive a client ID and a client secret. </p><p>In an Implicit flow, the client secret should never be exposed. </p> |
| **3. Resource Owner Password Grant**    |Does not have an login UI and is useful when access to a web browser is not possible.|            |
| **4. Client Credentials Grant**    | useful for machine to machine authorization.      |              |
 
Source: [[4]](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#4-developer-overview-of-openid-connect-onelogin-developers-online-available-httpsdevelopersonelogincomopenid-connect-accessed-jan-23-2023)

#### 3.4.2.3.1 Authentication flow in detail:

<kbd>![](https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/workflow_oidc.png)</kbd>
<p align=center>Fig: Workflow of OpenID Connect</p>

1. Login to your identity provider
2. The identity provider will provide you with an access_token, id_token and a refresh_token
3 When using kubectl, use your id_token with the --token flag or add it directly to your kubeconfig
4. kubectl sends your id_token in a header called Authorization to the API server
5 The API server will make sure the JWT signature is valid by checking against the certificate named in the configuration
6. Check to make sure the id_token hasn't expired
7. Make sure the user is authorized
8. Once authorized the API server returns a response to kubectl
9. kubectl provides feedback to the user

Source: [[3]](https://github.com/dikshita-git/Research-Project/wiki/Project-Report#3-authenticating-kubernetes-online-available-httpskubernetesiodocsreferenceaccess-authn-authzauthentication-accessed-jan-18-2023)
 


## 4. How OpenID Connect can improve the security of kube-apiserver?

Securing kube-apiserver with OpenID Connect will help us solve a large number of problems we had with the long-lasting and non-revocable certificates. This is due to the fact that:

- With OpenID Connect, signed tokens are now used for authentication and authorization instead of ever-lasting client-certificates.
- These tokens are short-lived and expire after a configurable duration which can be configured ourselves.
- We can also disable renewal of the tokens as per accordance.




--------------------------------------------------------------
## 5. Adoption of OIDC

<kbd>![Fig: State_of_art](https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/state_of_art.png)</kbd>


-------------------------------------------------------------


## 6. OpenID Connect with Keycloak in Practice

In order to implement OpenID Connect in our kubernetes cluster, Keycloak is used as the Identity Provider which will issue us the access token, id_token, refresh_token etc. A kubectl plugin called Kubelogin is also used to provide an ease to automatically open the browser with the login form of Keycloak when we try to query for kubernetes resources using kubectl. The workflow would look the image shown below:

<kbd><img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/kubelogin.png" width="800"  align="center"></kbd>
<p align=center>Fig: Authorization Code flow with kubelogin</p>

1. An operator queries for kubernetes resources in kubectl

2. kubectl runs the kubelogin plugin
3. kubelogin in returns automatically opens the browser consisting of the login form of keycloak
4&5. Once we successfully login with the username and password, browser sends an authentication request to keycloak to authenticate the user with those credentials.
6.  Keycloak in returns sends an Authorization code to browser
7. Browser forwards the authentication response i.e. this code to kubelogin
8. Now kubelogin requests an access token from keycloak on behalf of the authorization code.
9. Keycloak in return gives kubelogin the access token.
10. Once it receives the access token, it forwards to kubectl
11. Kubectl will then request kube-apiserver with this access token so that kube-apiserver can get the keycloak certificate associated with that token.
12&13. Kube-apiserver then gets the certificate from keycloak.
14. Now, kube-apiserver sends kubectl a response that the user is authenticated and is authorized to perform his actions.
15. kubectl forwards this message to user by displaying him the response to his queries. 



### 6.1 Steps of implementation :

These are the practical implementations which were performed on our VM and are included in the descriptive folder of [OpenIDconnect_practical](https://github.com/dikshita-git/Research-Project/tree/main/OpenIDconnect_practical) of this repository. Below is a short roadmap of the same:

- We provisioned our VM and created a cluster using kubeadm. 
   
    :arrow_right: [01_vm_setup_and_kubeadm.md](https://github.com/dikshita-git/Research-Project/blob/main/OpenIDconnect_practical/01_vm_setup_and_kubeadm.md)

- Deployed keycloak and configured TLS for it. Also, configured a FQDN with nip.io for the domain name to be resolvable in- and outside of the cluster.

    :arrow_right: [02_keycloak_and_tls.md](https://github.com/dikshita-git/Research-Project/blob/main/OpenIDconnect_practical/02_keycloak_and_tls.md)

    * Once it is successfully configured, we can access the keycloak dashboard as shown in the figure below:

     <kbd><img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/https_keycloak.png" width="1000" align="center"> 
      </kbd>
       <p align=center>Fig: Keycloak dashboard</p>

- We then create a client called "kubernetes" and assign it the authentication flows :

    :arrow_right: [03_configure_keycloak.md](https://github.com/dikshita-git/Research-Project/blob/main/OpenIDconnect_practical/03_configure_keycloak.md)

    <kbd><img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/03_b_2.png" width="1000" align="center"> 
    </kbd>
    <kbd><img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/03_b_3.png" width="1000" align="center"> 
    </kbd>
    <p align=center>Fig: Creating client in keycloak</p>

    * Then we create an user called "testuser" as shown below:

    <kbd><img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/03_d_2.png" width="1000" align="center"> 
    </kbd>
     <p align=center>Fig: Creating user in keycloak</p>

- Manually retrieve the tokens 

    :arrow_right: [04_manually_get_tokens.md](https://github.com/dikshita-git/Research-Project/blob/main/OpenIDconnect_practical/04_manually_get_tokens.md)
    
     <kbd><img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/04_a_1.png" width="1000" align="center"> 
     </kbd>
     <p align=center>Fig: Endpoints for tokens of OIDC received in JSON format</p>

- Configuring the kube-apiserver of our cluster in accordance with the keycloak claims.

   :arrow_right: [05_configure_kube-apiserver.md](https://github.com/dikshita-git/Research-Project/blob/main/OpenIDconnect_practical/05_configure_kube-apiserver.md)

- Configuring Role-based Access Control in cluster for the role created in keycloak

   :arrow_right: [06_configure_rbac.md](https://github.com/dikshita-git/Research-Project/blob/main/OpenIDconnect_practical/06_configure_rbac.md)

- Install kubelogin and test the OpenID Connect authentication

   :arrow_right: [07_kubelogin_set_up_and_test.md](https://github.com/dikshita-git/Research-Project/blob/main/OpenIDconnect_practical/07_kubelogin_set_up_and_test.md)



---------------------------------------------

## 6. Results

- As for testing purpose, I tried to access the kubernetes resources from my local machine with the command:

```
kubectl get pods
```
which in return opens up the browser for login as shown below:

<kbd><img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/k_get_pods.png" align="center"></kbd>
<p align=center>Fig: Redirection to browser for k get pods</p>

- We are now redirected automatically to the browser with the keycloak login form as shown below:

<kbd><img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/https_keycloak.png" width="1000" align="center"> 
</kbd>
<p align=center>Fig: Keycloak login</p>

- Once we successfully enter our credentials we will be redirected to our localhost with an "Authenticated" message as shown below:

<kbd><img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/success.png" width="1000" align="center"> 
</kbd>
<p align=center>Fig: Authentication successful message</p>

- Now we can see our pods running inside the cluster listed in our kubectl as below:

<kbd><img src="https://github.com/dikshita-git/Research-Project/blob/main/Project_report_images/pods_list.png" width="1000" align="center"> 
</kbd>
<p align=center>Fig: Pods inside clusters are listed</p>


------------------------

## 8. Summary

Using OpenID Connect, we have several benefits as in terms of security measures because of its short-lived tokens which can be easily revoked any time and the revocation timestamp can be configured as per our choice. Additionally, fine granular authentication and authorization management is also achieved with the integration of OpenID Connect with keycloak. Although OIDC can be implemented using the enterprise version of ingress controllers like nginx, traefik etc. which are exclusively meant for industries. In relation to the project, the goal was to deploy it in our bare-metal in the most simple ways with less cost for small to medium-size application organizations. However the implementation is definitely a complex setup besides learning the vast specification lists and grant flows of OAuth 2.0, OpenID Connect and various concepts of keycloak. 
Thus,we found a way to recover from the compromised client-certificates and client-keys and to mitigate the impact of cryptojacking attack. Further we implicitly unlocked features in keycloak like Two-Factor Authentication, password recovery and more.  


------------------------

## 9. Literature

##### [1] “An Illustrated Guide to OAuth and OpenID Connect,” Okta Developer. [Online]. Available: https://developer.okta.com/blog/2019/10/21/illustrated-guide-to-oauth-and-oidc. [Accessed: Jan. 23, 2023] 

##### [2] A. Mizrachi, “Demystifying OAuth Flows,” Frontegg, Oct. 28, 2021. [Online]. Available: https://frontegg.com/blog/oauth-flows. [Accessed: Jan. 23, 2023]

##### [3] Authenticating,” Kubernetes. [Online]. Available: https://kubernetes.io/docs/reference/access-authn-authz/authentication/. [Accessed: Jan. 18, 2023]

##### [4] “Developer Overview of OpenID Connect,” OneLogin Developers. [Online]. Available: https://developers.onelogin.com/openid-connect. [Accessed: Jan. 23, 2023]

##### [5] “Endpoints | ZITADEL Docs.” [Online]. Available: https://www.zitadel.com/docs. [Accessed: Jan. 23, 2023]

##### [6] “JSON Web Key Set Endpoint - Identity Server 5.4.1 - WSO2 Documentation.” [Online]. Available: https://docs.wso2.com/display/IS541/JSON+Web+Key+Set+Endpoint. [Accessed: Jan. 23, 2023]

##### [7] “IBM Documentation.” [Online]. Available: https://www.ibm.com/docs/en/sva/9.0.5?topic=SSPREK_9.0.5/com.ibm.isam.doc/config/concept/OAuthEndpoints.htm. [Accessed: Jan. 23, 2023]

##### [8] “Reibungslose Kubernetes OpenID Connect-Integration | Gini,” Apr. 18, 2018. [Online]. Available: https://gini.net/blog/reibungslose-kubernetes-openid-connect-integration/. [Accessed: Jan. 23, 2023]

##### [9] “Kubelogin Github.” [Online]. Available: https://github.com/int128/kubelogin. [Accessed: Jan. 19, 2023]
