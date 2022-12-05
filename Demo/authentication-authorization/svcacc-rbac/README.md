## Authenticating kubernetes based on service token and Role-based access control

A special thanks to <a href="https://hub.docker.com/r/bibinwilson/docker-kubectl">Bibin Wilson</a> for the reference to kubectl utility image used in this experiment.

## Table of Contents:

<a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/README.md#introduction">1. Introduction</a>

<a href="">2. Kubernetes access control types</a>

<a href="">3. Steps</a>


### Introduction:

Allowing limited access to kubernetes resources are a foundation ot the security of the cluster thus protecting it from malwares and phishing attacks. By default, kubernetes uses client certificates, bearer tokensor authentication proxy in order to authenticate the incoming API requests through the authentication plugins. Authentication in kubernetes validation of ***"who"*** and ***"what"*** is issuing the requests. Parallel to it, authorization means ***"verifies whether a certain action to list pods or services is allowed or permitted to a certain user(under a certain namespace may be)."***

Usually the API server authenticates every incoming human-made request (not a program/pod request). By default kubernetes does not maintain any database to store the credentials of the users. This means that it expects it to be managed by an external source. Thus, the human user accounts have to be managed by one of the supported authentication strategies → <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#there-are-various-kubernetes-built-in-api-authentication-process-which-are-suitable-only-for-smaller-clusters-or-at-the-staging-level-which-are-intended-to-implement-only-specific-authentication-methods-thus-they-can-also-be-regarded-as-closed-end-authentication-plugins">Read here</a>.

Kubernetes provides two main access control choices namely:

### 1. Role-Based Access Control (RBAC):

It is a method of regulating the access to the kubernetes resources and enables us to configure fine-grained and specific set of permissions which will specify what or how a mentioned user or a group can/should perform. ***Verbs*** define the actions like "get", "list", "watch", "create", "update", "patch", "delete" that should be performed

When we deploy pod using <code>kubectl apply -f </code>command, a few things happen behind the scenes:

i) Reads the configs from your KUBECONFIG, 

ii) Discovers APIs and objects from the API, 

iii) Validates the resource client-side (is there any obvious error?), 

iv) Sends a request with the payload to the kube-apiserver. 

When the kube-apiserver receives the request, it doesn't store it in etcd immediately. First, it has to verify that the requester is legitimate. In other words, it has to authenticate the request. Just because you have access to the cluster doesn’t mean you can create or read all the resources. So, once authenticated, it checks the requester have permission to create the resource. In other words, check request authorization. The authorization is commonly done with Role-Based Access Control (RBAC).

### 2. Attribute-Based Access Control (ABAC)

It is an authorization method in which evaluates attributes or characteristics inorder to determine the access. The administrator defines a set of user authorization policies into a file with one JSON per line format.THe main drawback of this method is that any modification in the file requires the API server to be restarted.

------------------------------------------------------------------------------------------------------------------------------------------------

This experiment is conducted with RBAC and service account.


--------------------------------------------------------------------------------------------------------------

### STEPS:

#### 1. Verify if RBAC is enabled in the kubernetes version :

Before proceeding, it is recommended to check the RBAC if enabled in order to get the version number as it might change as per kubernetes version.

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/images/check_rbac_enabled_new.png">
<i>Fig: Verify the RBAC </i>


#### 2. Create serviceaccount:

Kubernetes mainly has two categories of users: Useraccount/normal users and serviceaccount.


|  Useraccount  | Serviceaccount |
| ------------- | -------------- |
| 1. Human users | 1. Kubernetes internal account  |
| 2. Allows humans to access the kubernetes cluster  | 2. Provides an identity for process that runs in a pod  |
| 3. Used by administrators and developers to access the cluster and develop something or for maintenance.| 3.   Used by applications and process in order to authenticate the process to get access to our cluster. API server authenticates the process running in pod.    | 


#### Working of serviceaccount:

#### Case 1: When you have an external application trying to access Kubernetes cluster API servers.


#### Case 2: When the application is hosted and running within the Cluster POD (our case)


For this experiment, a read-only serviceaccount named "sa" is created which has access to only the "default" namespace of the kubernetes cluster. This is done using the command:

```
k3s kubectl create serviceaccount sa -n default
```

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/images/2.png">
<i>Fig: Create serviceaccount</i>


#### 3. Extract the service token created:

In more recent versions, including Kubernetes v1.25, API credentials are obtained directly by using the TokenRequest API in comparison to versions of kubernetes before v1.22 automatically created long term credentials for accessing the Kubernetes API. This older mechanism was based on creating token secrets that could then be mounted into running Pods. But we can also create a serviceaccount token secret manually by creating a <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/secret.yaml">secret.yaml</a></code>.
Because of the annotation we set in this secret.yaml, the control plane automatically generates a token for that serviceaccount, and stores them into the associated secret. The control plane also cleans up tokens for deleted serviceaccounts.
We deploy this file using the command:

```
k3s kubectl apply -f secret.yaml
```

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/images/3.png">
<i>Fig: Create secret.yaml</i>


Now, we can see the token which is generated using the command:

```
k3s kubectl get secret/sa-token -o yaml
```

Here we have to specify secret/<name_of_secret>. In this case, the secret name is "sa-token".

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/images/4_secret-token.png">
<i>Fig: Extract secret token</i>


#### 4. Create a role:

RBAC authorization defines permissions or policies to limit the access to the cluster resource. This set of permissions is called "Role" and is defined within a namespace. On the contraast, ClusterRole is a non-namespaced resource.

Now we create role (<code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/default-role.yaml">default-role.yaml</a></code>) with the name "sarbac" with different permissions. Here the actions: verbs: ["get", "list", "watch"] can be performed for the resources : ["pods"] only in the "default" namespace. THen deploy the file suing:

```
k3s kubectl apply -f default-role.yaml
```

#### 5. Create a role binding:

Role binding in RBAC in kubernetes grants the permission that is defined in the role for the users or group of users. It consists of a set of subjects namely users, groups or serviceaccounts and a reference to the role that is being granted. Rolebinding grants the permission within a specified namespace whereas ClusterRolebInding grants the permission cluster-wide. In this demo, I have used role binding only.

This role created should now be binded to the serviceaccount "sa" by this rolebinding. This is necessary in order to make sure that the serviceaccount "sa" can only ["get", "list", "watch"]  pods in the "default" namespace. Here is my <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/role-binding.yaml">role-binding.yaml</a></code>. Deploy the file:

```
k3s kubectl apply -f role-binding.yaml
```
We can check the rolebindings with the command:

```
k3s kubectl get rolebindings -n <namespace_name>
```

as shown in the image below

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/images/6.png">
<i>Fig: Check rolebindings </i>

#### 6. Set an user and token in kubeconfig:

The secret token we retrieved in step 3 should now be set to the config credentials. Here I used "masterdev" as the username which is my VM name.

```
k3s kubectl config set-credentials <user_name> --token=<paste_the_secret_token_here>
```
My command is shown in the image below:

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/images/4_secret-token.png">
<i>Fig: Set user and token </i>


#### 7. Set test permissions:

For this specific step, the docker image <code>bibinwilson/docker-kubectl</code> is used which has a kubectl command inside.  
Now we create a <code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/pod.yaml">pod.yaml</a></code> named "sa-test" which allows to perform only the functions ["get", "list", "watch"] pods in the default namespace and assigned the serviceaccount "sa" to it. To deploy this pod:

```
k3s kubectl exec -it --namespace=<namespace_name> <pod_name> -- /bin/bash
```
Here is the image reference:

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/images/9.png">
<i>Fig: Apply pod </i>

This command will forward us to the bash script of the "sa-test" and here we can test our access to the pod. For this purpose, I have created another namespace called "test-sa" which will try to access the pod to check if our permissions are set right.

```
k3s kubectl get pods -n test-sa
```

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/images/success_screen.png">
<i>Fig: Test permission to pod </i>

