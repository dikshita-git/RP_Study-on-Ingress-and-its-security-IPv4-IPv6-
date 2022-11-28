This practical demonstration is conducted with an aim to authenticate my kubernetes web app dkrp3.xyz using Open ID connect as the authentication plugin and keycloak as the identity provider and token issuer.

## STEPS :

As a mandatory and a considerable prerequisite for the demonstration, a k3s cluster with multi-worker is created which has the following nodes with their IP addresses:

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/keycloak/image/get_nodes.png">
<p>Fig: Node check</p>

## 1. Create a namespace called keycloak:

The namespace "keycloak" is created in order to organize my keycloak deployments under this ns by the command.

```
k3s kubectl create ns keycloak
```

## 2. Create a deployment file:

This <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/keycloak/deployment.yaml">deployment file</code>  basically creates a deployment named "keycloak" and 2 replica pods of it with the specifications of the official container image of keycloak and the keycloak credentials are given as input under "values". The port 8080 specifies that once our pods get created, we can access tehm using their IP address appendig this port number.
 

## 3. Create a service file:

This <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/keycloak/service.yaml">service file</code> is a loadbalancer type.
  
The deployment.yaml and service.yaml will deploy 3 keycloak pods.
  

## Deploy traefik ingress:
  
For this experiment, we would want to use traefik as the ingress to our domain dkrp3.xyz and this requires an easy set up using helm. The folllowing steps were carried out:

```
 1. curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
 2. chmod 700 get_helm.sh
 3. ./get_helm.sh 
 4. helm repo add jetstack https://charts.jetstack.io   /**adding helm repo**/
 5. helm repo update   /**Update your local Helm chart repository cache:**/
```
  
  
## 5. Check pods and curl them
 
Now, we can check our pods using the command:
  
```
k3s kubectl get pods -o wide
```
  
This leads to the details of the pods created which further enables us to check if the keycloak we is accessible. If yes, then we can now check the browser with the https://[IP_address_of_any one of listed pod]:8080 and we access the dashboard of keycloak which we access using the username and password as declared in the <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/keycloak/deployment.yaml">deployment.yaml</a></code> file.
  
<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/keycloak/image/get_pods_o_wide.png">
<p>Fig: Pod check</p>
  
<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/keycloak/image/curl.png">
<p>Fig: Curl pod</p>
  

