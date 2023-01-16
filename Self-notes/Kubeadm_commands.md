### Install kubernetes:
1. curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add

2. sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

3. sudo apt update

4. sudo apt install kubeadm kubelet kubectl

5. sudo apt-mark hold kubeadm kubelet kubectl

6. /First diasbale swap/ sudo swapoff -a

7. kubeadm init

8. mkdir -p $HOME/.kube

9. cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

10. chown $(id -u):$(id -g) $HOME/.kube/config


#### Possible error:
```
unknown service runtime.v1alpha2.RuntimeService"
```
#### Solution:
rm /etc/containerd/config.toml
systemctl restart containerd
kubeadm init
