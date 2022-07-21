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
-------------

- To install I used the script by Rancher as given below:

[Rancher Script]
$ curl https://releases.rancher.com/install-docker/19.03.sh | sh

2. Kubectl
-------------

- We need to set up kubectl to communicate with the Kubernetes API server.

$ curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"

$ chmod +x ./kubectl

$ sudo mv ./kubectl /usr/local/bin/kubectl

We can check the installation by: 

kubectl version --client

3. K3D
-------------

wget -q -O - https://raw.githubusercontent.com/rancher/k3d/main/install.sh | bash

***NOTE.*** There might be certain errors when we would try to create a cluster (k3d cluster create "Name") such as tools missing, could not find volumes etc. In this case just upgrade the docker version with the command (apt upgrade docker-ce) and it works.






Here the SSH keys and GPG keys are established and synchronised with my machine. The GIT commands used thereby are:

### GIT PUSH:
  1. Navigate inside the folder (could be the cloned repo too)  which has the new updates to be pushed to GIT 
  2.  git add . -->in case of adding the whole folder or git add <Folder_Name>
  3.  git status ---> to check the updates
  4.  git commit -m "Message"
  5.  git remote set-url origin <SSH_Link_of Git_repo>
  6.  git push
### GIT PULL:
  1. mkdir 
  2. git clone <ssh_repo_link>
 
PS: OpenSSh server could be installed on the machine beforehand because the latest GIT operations hold right only either via SSH or Authentication apps like Twilio Authy etc.

### PORTRAINER: (installed as)
  1. root@ubuntu:/home/dk/Desktop# docker volume create portainer_data
  2. root@ubuntu:/home/dk/Desktop# docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
  
      ***Check http://localhost:9000/#/home***
 
 
 ### MINIKUBE: (installed as)
  1. As root : minikube start --driver=none
  2. <i>Minikube dashboard command</i>: minikube dashboard
  3. In my Ubuntu, alwasy start minikubeusing docker driver on: <b>dikshita@ubuntu</b> with the command: <b>minikube start  --driver=docker</b>


***NOTE: Always stop minikube by doing: "docker system prune"***


### DIGITAL OCEAN:
   1. doctl auth init (To start)
   2. doctl kubernetes cluster kubeconfig save demo-cluster-1 (To save a particular cluster to be accessed)
   3. kubectl --kubeconfig=~/.kube/demo-cluster-1-kubeconfig.yaml get nodes (Checkk nodes in that cluster)

---------------------------------------------------------------------------------------------------------------------------------------------------------------

# <u>My GIT Roadmap:</u>


Here is a roadmap to the files in this repository:
 ## Wiki  : 
 It consists of all the documentations of ideas gathered throughout:
 
1. <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Daily_MoM">Weekly meeting table</a>
        
2. <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Docker-%7C-Its-Environments">Docker</a>

3. <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Fundamentals-of-K8s">Fundamentals of K8s</a>

4. <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Ingress">Ingress</a>

5. <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Traefik">Traefik</a>
 
 ## Codes  : 
        Folder with all the code files
