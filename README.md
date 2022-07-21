## RP_Study-on-Ingress-and-its-security-IPv4-IPv6-
This is an academic Research project carried out for intensive learning of Ingress across different platforms and determines the possible measures to secure it both using IPv4 and IPv6.

### K3s:
This is a lightweight distribution of Kubernetes and we can easily create clusters and manage it using the tool K3d which basically means K3s with Docker.

To install k3d we need these requirements:
1. 512MB RAM (server)
2. 75MB RAM (node)
3. 200MB disk space range

Installation requirements:

1. Docker
=========

- To install I used the script by Rancher as given below:

[Rancher Script]
$ curl https://releases.rancher.com/install-docker/19.03.sh | sh

2. Kubectl
=========

- We need to set up kubectl to communicate with the Kubernetes API server.

$ curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"

$ chmod +x ./kubectl

$ sudo mv ./kubectl /usr/local/bin/kubectl

We can check the installation by: 

kubectl version --client

3. K3D
=========

wget -q -O - https://raw.githubusercontent.com/rancher/k3d/main/install.sh | bash

***NOTE.*** There might be certain errors when we would try to create a cluster (k3d cluster create "Name") such as tools missing, could not find volumes etc. In this case just upgrade the docker version with the command (apt upgrade docker-ce) and it works.
