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
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key
add
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo apt update
sudo apt install kubeadm kubelet kubectl
sudo apt-mark hold kubeadm kubelet kubectl
kubeadm init
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config


### Remove kubernetes:
kubeadm reset
sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*
sudo apt-get autoremove
sudo rm -rf ~/.kube


### Install k3s:
curl -sfL https://get.k3s.io | sh - 
k3s kubectl get node 
