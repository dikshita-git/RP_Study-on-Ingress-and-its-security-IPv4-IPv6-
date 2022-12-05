## Authenticating kubernetes based on service token and Role-based access control

A special thanks to <a href="https://hub.docker.com/r/bibinwilson/docker-kubectl">Bibin Wilson</a> for the reference to kubectl utility image used in this experiment.

Allowing limited access to kubernetes resources are a foundation ot the security of the cluster thus protecting it from malwares and phishing attacks. By default, kubernetes uses client certificates, bearer tokensor authentication proxy in order to authenticate the incoming API requests through the authentication plugins. Authentication in kubernetes validation of ***"who"*** and ***"what"*** is issuing the requests. Parallel to it, authorization means ***"verifies whether a certain action to list pods or services is allowed or permitted to a certain user(under a certain namespace may be)."***

Usually the API server authenticates every incoming human-made request (not a program/pod request). By default kubernetes does not maintain any database to store the credentials of the users. This means that it expects it to be managed by an external source. Thus, the human user accounts have to be managed by one of the supported authentication strategies <a href="https://github.com/dikshita-git/Research-Project/wiki/Project-Report#there-are-various-kubernetes-built-in-api-authentication-process-which-are-suitable-only-for-smaller-clusters-or-at-the-staging-level-which-are-intended-to-implement-only-specific-authentication-methods-thus-they-can-also-be-regarded-as-closed-end-authentication-plugins">Read here</a>x


### STEPS:

#### 1. Verify if RBAC is enabled in the kubernetes version :

Before proceeding, it is recommended to check the RBAC if enabled in order to get the version number as it might change as per kubernetes version.

<img src="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/images/check_rbac_enabled_new.png">
<i>Fig: Verify the RBAC </i>


#### 2. Create serviceaccount:

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

Now we create role (<code><a href="https://github.com/dikshita-git/Research-Project/blob/main/Demo/authentication-authorization/svcacc-rbac/default-role.yaml">default-role.yaml</a></code>) with the name "sarbac" with different permissions. Here the actions: verbs: ["get", "list", "watch"] can be performed for the resources : ["pods"] only in the "default" namespace. THen deploy the file suing:

```
k3s kubectl apply -f default-role.yaml
```

#### 5. Create a role binding:

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

