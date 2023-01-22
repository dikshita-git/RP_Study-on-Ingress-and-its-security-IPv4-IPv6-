
## Configuring Keycloak

Please click the link <a href="https://github.com/dikshita-git/Research-Project/tree/main/OpenIDconnect_practical/03">here</a> to see the screenshots as per the steps:


#### STEPS:

***a)***

first open https://10.20.116.209.nip.io
click Administrator Console

***b)***

create client
Administrator Console -> Clients -> Create client
	Client ID: kubernetes

-> next
	Client Authentication: Yes
	Standard Flow: Yes
	Direct Access Grants: Yes
	-> leave others default
	-> save

-> set valid redirect uris to
	http://localhost:8000
	-> this is a client specific setting
	-> in our case we use kubelogin
	-> this means kubelogin will listen on localhost:8000 to accept the redirect with the tokens

***c)***

create client role
(for mapping to user later)
Inside client "kubernetes"
	-> Tab "roles"
	-> Create role
		Role Name: kubernetes_role
	-> Save

***d)***

create testuser

Administrator Console -> Users -> Add user
	Username: testuser
	Email: testuser@testuser.de
	E-Mail verified: On
	First name: firstname
	Last name: lastname
	-> create

***e)***

configure user, set credentials and add to client role

-> Tab "Credentials" -> Set password
	Password: testpass
	Password confirmation: testpass
	Temporary: Off
	-> save
	-> again Save password

-> Tab "Role mapping" -> Assign role
	-> click "Filter by realm roles"
	-> change to "Filter by clients"
	-> select "kubernetes_role"
	-> Assign 

💡 ***Status:***

	- now we can do "Direct Access Grant" and "Authorization Code Flow"
	- but we might not have the correct claims

***f)***

add mapper inside client scope 
so that we get the groups claim in our token payload
this will be later used to be evaluated to kubernetes groups via kube-apiserver


-> configure kubernetes client
	Tab "Client scopes"
	-> click on kubernetes-dedicated
	-> Add predefined mapper
	-> select "client roles" (this will map client roles to a claim)

-> click on new mapper "client roles" to configure it
	-> change "Token Claim Name" to "groups"
	-> Save

this will now show up in our jwt payload:
  "groups": [
    "kubernetes_role",
    "manage-account",
    "manage-account-links",
    "view-profile"
  ],
