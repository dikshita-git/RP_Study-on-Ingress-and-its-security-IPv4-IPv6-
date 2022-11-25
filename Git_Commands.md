# Next meeting :13 dec,14:00



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


### Remove kubernetes:
1. kubeadm reset
2. sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*
3. sudo apt-get autoremove
4. sudo rm -rf ~/.kube


### Install k3s:
1. curl -sfL https://get.k3s.io | sh - 
2. k3s kubectl get node 



💡NOTE: We might need to delete the <code>certificate-authority-data:</code> from /etc/kubernetes/admin.conf if an error of "Unable to connect to server.X509 certificate is valid for these ip address" persists.
