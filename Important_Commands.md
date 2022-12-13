# Final meeting: 20th January, Friday

Here the SSH keys and GPG keys are established and synchronised between my proxmox server and the github account. The GIT commands used thereby are:

#### 1. git init
#### 2. git config --global user.name "John Doe"
#### 3. git config --global user.email "John Doe@gmail.com"


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


### Instal docker

1. curl -fsSL https://get.docker.com -o get-docker.sh
2. sudo sh get-docker.sh
 
 
### Install kubernetes:
1. curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
2. sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
3. sudo apt update
4. sudo apt install kubeadm kubelet kubectl
5. sudo apt-mark hold kubeadm kubelet kubectl
6. /***First diasbale swap***/
sudo swapoff -a
7. /***And then to disable swap on startup in /etc/fstab***/
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
8. kubeadm init
9. mkdir -p $HOME/.kube
10. cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
11. chown $(id -u):$(id -g) $HOME/.kube/config
12. Install helm:
   
        1. curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
  
        2. chmod 700 get_helm.sh
 
        3. ./get_helm.sh

13. Update helm repo

        helm repo update
   
14. Install CRDs

        kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.10.1/cert-manager.crds.yaml


### Remove Docker:

1. sudo apt-get purge -y docker-engine docker docker.io docker-ce  
2. sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce  
3. sudo umount /var/lib/docker/
4. sudo rm -rf /var/lib/docker /etc/docker
5. sudo rm /etc/apparmor.d/docker
6. sudo groupdel docker
7. sudo rm -rf /var/run/docker.sock
8. sudo rm -rf /usr/bin/docker-compose
9. apt-get purge -y docker-engine docker docker.io docker-ce docker-ce-cli


### Remove kubernetes:
1. kubeadm reset
2. sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*
3. sudo apt-get autoremove
4. sudo rm -rf ~/.kube


### Install k3s:
1. curl -sfL https://get.k3s.io | sh - 
2. k3s kubectl get node 

### Install Minikube:

1. apt update -y
2. reboot
3. apt install -y curl wget apt-transport-https
4. wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
5. cp minikube-linux-amd64 /usr/local/bin/minikube
6. chmod +x /usr/local/bin/minikube

##### Install cri-dockerd

8. systemctl status docker
9. apt update

# 10. Add user to docker
# password@Add User to the Docker Group

* sudo groupadd docker
* sudo usermod -aG docker $USER


### - Re-Login or Restart the Server

#### Install Minikube

* curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

* chmod +x minikube

* mv ./minikube /usr/local/bin/minikube

#### Start minikube with Docker Driver

minikube start --driver=docker

💡***NOTE:***

 In case of errors of using drivers in minikube while minikube start , remove all the unused containers, network images both dangling and unreferenced and volumes using the following:
 
```
1. docker system prune
2. minikube delete
3. minikube start --driver=docker

```


💡***NOTE:***
1. We might need to delete the <code>certificate-authority-data:</code> from /etc/kubernetes/admin.conf if an error of "Unable to connect to server.X509 certificate is valid for these ip address" persists.
2. Errors like ***container runtime problem*** may occur while doing ***kubeadm init*** in that case do: 

* rm /etc/containerd/config.toml
* systemctl restart containerd
* kubeadm init

### Possible errors:
1. Port in use:

       1. Install netstat with: apt install net-tools
       2. netstat -ltnp | grep -w ":10250"
       3. kill -9 <job_is(will be on the right)>
