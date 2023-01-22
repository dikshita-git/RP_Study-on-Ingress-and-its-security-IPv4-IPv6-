# Installing Keycloak and configuring its TLS


#### 1. install keycloak

```
  kubectl create namespace keycloak
  kubectl create -n keycloak -f https://raw.githubusercontent.com/keycloak/keycloak-quickstarts/latest/kubernetes-examples/keycloak.yaml
  k get all -n keycloak
  #kubectl apply -n keycloak -f nodeport_keycloak.yaml
```

#### 2. install helm
see https://helm.sh/docs/intro/install/

```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

#### 3. install ingress controller -> traefik or nginx-ingress are good choices

- the ingress controller
  - in a public cloud like the google cloud our ingress controller would start a loadbalancer which has a single IP
  - but we have only the two ip's of the VMs in our current cluster -> master and worker, which each one ip
  - there are no spare IP's to be used for a loadbalancer (which for example metallb could configured to use)
  - this means we need a way to provide ports 80 and 443 on the two VMs and use their existing ips
  - the solution
      - a daemonset is running on every node, but by default not on master nodes
      - we want to have a pod running on master awell, this means we need a toleration
      - we will install kubernetes-nginx (which is not the same as ingress-nginx)
      - and use host network for the pods so that they serve on the node's host network namespace
      - daemonset will have tolerations to also include master node
  - Why no deployment instead of a daemonset?
      - if the deployment schedules an ingress controller pod on the same node
      - then it would fail, because the ports 80 and 443 are only available once per node
      see https://github.com/kubernetes/ingress-nginx/blob/main/docs/deploy/baremetal.md

https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/

  - helm repo add nginx-stable https://helm.nginx.com/stable
  
  - helm repo update
  
```
$ cat << EOF > nginx-config.yaml
controller:
  kind: daemonset
  hostNetwork: true
  tolerations:
      - key: node-role.kubernetes.io/control-plane
        operator: Exists
        effect: NoSchedule
      - key: node-role.kubernetes.io/master
        operator: Exists
        effect: NoSchedule
EOF
```

```
helm install my-release nginx-stable/nginx-ingress -f nginx-config.yaml
```

- http requests should show error 404 now, a port is open on "ss -tlnp"
``` 
 curl 10.20.116.209:80
  curl 10.20.116.231:80
  ss -tln
```

#### 4. make a CA certificate (Files are <a href="https://github.com/dikshita-git/Research-Project/tree/main/OpenIDconnect_practical/myssl">here</a> in repo)


- create certificate manually and store it in a secret

- or create this secret automatically via cert-manager
  
  I choosed to create it manually -> no good reason, both works

- Certificate Authority = Private and Public Key, the latter being in a X.509 certificate
         
         - which has the CA: True flag set
         
         - therefore this privkey and certificate can sign other certificate
         
- generate CA for cert-manager or whatever it wants
- 
    source: https://medium.com/@charled.breteche/kind-keycloak-securing-kubernetes-api-server-with-oidc-371c5faef902
    
    ```
    # create a folder to store certificates
    mkdir -p myssl # generate an rsa key

    # create CA key, which can sign others -> first one
    openssl genrsa -out myssl/root-ca-key.pem 4096 # generate root certificate

    # create CA certificate, which can sign others
    openssl req -x509 -new -nodes -key myssl/root-ca-key.pem \
      -days 3650 -sha256 -out myssl/root-ca.pem -subj "/CN=kube-ca"
   ```
   
#### 5. create certificate for fqdn signed by our new CA

source: https://medium.com/@charled.breteche/kind-keycloak-securing-kubernetes-api-server-with-oidc-371c5faef902

```
k get -n keycloak service # obtain cluster ip of keycloak
export clusterip=10.96.57.131
export nodeip=10.20.116.209
export fqdn=${nodeip}.nip.io
```
```
curl ${nodeip}:8080 # nginx ingress controller is there, giving 404
curl ${clusterip}:8080 # keycloak answers
curl ${fqdn}:8080  # nginx ingress controller is there, giving 404
```
```
ping ${nodeip} # works, the node is pingable
ping ${clusterip} # does not work, clusterip is only made via iptables and no open port
ping ${fqdn} # works, is identical to node ip
```

- we create a config file for second certificate which cannot sign others

```
cat <<EOF > myssl/req.cnf
[req]
req_extensions = v3_req
distinguished_name = req_distinguished_name

[req_distinguished_name]

[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = ${fqdn}
EOF
```

- we create key for second certificate which cannot sign others

```
openssl genrsa -out myssl/key.pem 4096 # create certificate signing request
```

- we generate a Certificate Signing Request
- second certificate wants to be signed by first one

 ```
openssl req -new -key myssl/key.pem -out myssl/csr.pem \
  -subj "/CN=${fqdn}" \
  -addext "subjectAltName = DNS:${fqdn}" \
  -sha256 -config myssl/req.cnf # create certificate
```

- first one signs the second one and then we have a new certificate for the second one

```
openssl x509 -req -in myssl/csr.pem \
  -CA myssl/root-ca.pem -CAkey myssl/root-ca-key.pem \
  -CAcreateserial -sha256 -out myssl/cert.pem -days 3650 \
  -extensions v3_req -extfile myssl/req.cnf # create secret used by keycloak ingress
```

- verify

```
  ls myssl
  openssl rsa -in myssl/key.pem -check
  openssl x509 -in myssl/cert.pem -text -noout
```

#### 6. creating a secret (for the ingress) carrying the certificate and key

```
kubectl create -n keycloak secret tls keycloaktls \
  --cert=myssl/cert.pem \
  --key=myssl/key.pem
```

#### 7. create ingress

- proxy-buffer-size is important for keycloak
- with nginx and keycloak the limits would exceed and therefore need to be increased
- it could be found out by looking at the logs of the nginx ingress controller while trying to authenticate with keycloak

```
cat <<EOF > keycloak_ingress.yaml
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: keycloak-ingress
    namespace: keycloak
    annotations:
      nginx.org/proxy-buffer-size: 128k
      nginx.org/proxy-buffers: 4 256k
      nginx.org/proxy-busy_buffers-size: 256k
  spec:
    ingressClassName: nginx
    rules:
      - host: ${fqdn}
        http:
          paths:
            - pathType: Prefix
              backend:
                service:
                  name: keycloak
                  port:
                    number: 8080
              path: /
    tls:
      - hosts:
        - ${fqdn}
        secretName: keycloaktls
EOF
k apply -f keycloak_ingress.yaml
```
- Test with curl

```
curl --insecure https://${fqdn} # should work on node and vm host, and in GUI browser on vm host
https://10.20.116.209.nip.io
```
:arrow:right: ***Summary:***

Now we have a working keycloak instace accessible over both our master and worker node's ip and it provides tls with a self-signed certificate, which is signed for the master node's ip
