### Install kubernetes:
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo apt update
sudo apt install kubeadm kubelet kubectl
sudo apt-mark hold kubeadm kubelet kubectl
/First diasbale swap/ sudo swapoff -a
/And then to disable swap on startup in /etc/fstab/ sudo sed -i '/ swap / s/^(.*)$/#\1/g' /etc/fstab
kubeadm init
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown 
(id -g) $HOME/.kube/config
#### Possible error:
```
unknown service runtime.v1alpha2.RuntimeService"
```
#### Solution:
rm /etc/containerd/config.toml
systemctl restart containerd
kubeadm init
