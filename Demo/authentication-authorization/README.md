This practical demonstration is conducted with an aim to authenticate my kubernetes web app dkrp3.xyz using Open ID connect as the authentication plugin and keycloak as the identity provider and token giver.

STEPS :

## 1. Create a namespace called keycloak:

The namespace "keycloak" is created in order to organize my keycloak deployments under this ns by the command.

```
k3s kubectl create ns keycloak
```

## 2. Create a deployment file:

This <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/deployment.yaml">deployment file</code>  basically creates a deployment named "keycloak" and 2 replica pods of it with the specifications of the official container image of keycloak and the keycloak credentials are given as input under "values". The port 8080 specifies that once our pods get created, we can access tehm using their IP address appendig this port number.

  
  
## 3. Create a service file:

This <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/service.yaml">service file</code> is a loadbalancer type.
  
the deployment.yaml and service.yaml will install keycloak in my k3s cluster.
  
  
## 4. Check pods and curl them
 
Now, we can check our pods using the command:
  
```
k3s kubectl get pods -o wide
```
  
THis leads to the details of the pods created which further enables us to check if the keycloak we is accessible. If yes, then we can now check the browser with the https://[IP_address_of_pod]:8080 and we access the dashboard of keycloak to set the username and password.
  
<img src="https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/keycloak-pods.png">
<p>Fig: Pod check</p>
  
<img src="https://github.com/dikshita-git/Research-Project/blob/main/Wiki-page-images/Research_Question/keycloak_curl.png">
<p>Fig: Curl pod</p>
