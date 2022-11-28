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
  

## 4. Deploy traefik ingress:
  
For this experiment, we would want to use traefik as the ingress to our domain dkrp3.xyz and this requires an easy set up using helm. The folllowing steps were carried out:

```
 1. curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
 2. chmod 700 get_helm.sh
 3. ./get_helm.sh 
 4. helm repo add jetstack https://charts.jetstack.io   //Adding helm repo//
 5. helm repo update                                   //Update your local Helm chart repository cache://
 6. kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.10.1/cert-manager.crds.yaml         ##Install CRDs//
 7. helm install \
      cert-manager jetstack/cert-manager \
      --namespace cert-manager \
      --create-namespace \
      --version v1.10.1                                      //Install cert-manager//
```
Once these steps are completed, we deploy our <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/keycloak/keycloak_ingress.yaml">keycloak-ingress.yaml</a></code> and to store the certificates the <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/keycloak/traefik_config.yaml">traefik_config.yaml</a></code>.
  
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
 
 
## 6. SSH tunnelling to access keycloak:

This step of local forwarding is optional and is recommended only when the VM has no GUI feature to access the browser. In this case, the working VM IP was ***masterdev@10.20.116.213*** and the IP of keycloak pod that we want in to access in a browser are: ***10.42.1.27, 10.42.2.19, 10.42.0.15***, thus we run the command below in our VM terminal where we have our browser access:

```
ssh -L 8080:10.42.1.27:8080 masterdev@10.20.116.213
```
This would tunnel us to accessing our keycloak dashboard using http://localhost:8080/ which looks like below (afetr signing in via Keycloak admin console):

<img src="<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/keycloak/image/get_pods_o_wide.png">
<p>Fig: Keycloak dashboard in localhost</p>">

 
## 7. Create Realm:
 
By default, ***master*** Realm is created which is basically the admin user which we created in our deployment file. Realm in keycloak is an object that manages users/set of users, its credentials, roles and groups. It can be thought of having a similar way of functionality like a namespace holding different resources and groups in it. Here, to create the Realm, we followed:

```
1. Click on the dropdown near "master" realm on the side-menu on left.
2. Click "Create Realm"
3. Give a realm name at → Realm-name:myrealm
4. Hit Save
```
 
<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/keycloak/image/create_realm.png">
<p>Fig: Creating keycloak Realm</p>

## 8. Create User:
Once a realm is created, now we can assign users to it which  can be further used to login to our kubernetes application. In this case, we craete a user named ***dk*** by following the steps below:

```
1. Click on the "Users" tab under myrealm on the side-menu on left.
2. Click "Create User"
3. Give a username at → Username:dk
4. Give First Name
5. Give Last Name
6. Hit Save
```
<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/keycloak/image/create_user.png">
<p>Fig: Creating user</p>
