# Installing kubectl plugin "kubelogin" to do OIDC athentication and test it

For this purpose, I installed kubectl and kubelogin in my laptop (Arch linux) and the i tried to access the kubernetes resource of my cluster running on server(master2). The purpose of kubelogin here would be that it will automatically open up a browser with the login form of keycloak where we will enter the credentials to get authenticated.


#### 1. install kubelogin on host

```
yay -S kubelogin kubectl
```

#### 2. trust custom CA on host

- copy ca file to host
- then

```
sudo trust anchor --store root-ca.pem
```

#### 3. set up kubelogin

- extend kubeconfig on NODE with new context

- there are several ways, but for simplicity:

    - server


```
        mkdir /home/master2/.kube
        cp /root/.kube/config /home/master2/.kube/config
        chown -R master2:master2 /home/master2/.kube
```

    - laptop
 
```
        mv ~/.kube/config kube_config_backup
        scp master2@10.20.116.209:~/.kube/config ~/.kube/config
 ```
 
- next add a new user to the kube config file:

  - vi ~/.kube/config

```

users:
- name: oidc
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      command: kubectl
      args:
      - oidc-login
      - get-token
      - --oidc-issuer-url=https://10.20.116.209.nip.io/realms/master
      - --oidc-client-id=kubernetes
      - --oidc-client-secret=FPltK3k7eA5agTbnTmwc0ounthRVVNaW
```

#### 4. switch context on vm host

- now change user to the new one on laptop:

```
    kubectl config set-context --current --user=oidc
```

#### 5. try to access resource

- :arrow-right: k get pods
(browser should open for auth)

- debugging

   - log out in keycloak via account console (so it stops redirecting)
   
   https://10.20.116.209.nip.io/realms/master/account
    
   - delete already obtained token
	
	rm -r ~/.kube/cache/oidc-login
	
   - token can be inspected via

```
	$ kubectl oidc-login setup \
		--oidc-issuer-url https://10.20.116.209.nip.io/realms/master \
		--oidc-client-id kubernetes \
		--oidc-client-secret FPltK3k7eA5agTbnTmwc0ounthRVVNaW
```

- being logged in as admin gives admin token, which has wrong claims!
- :red_circle: error message when token is wrong
- 
	error: You must be logged in to the server (Unauthorized)

#### 6. reset context to normal admin user

:arrow_right: kubectl config set-context --current --user=kubernetes-admin
