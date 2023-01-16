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


### Join workers to master

1. ***In worker:***  kubeadm join 10.20.116.209:6443 --token 2fxg38.7tx3ph8hptzv33no \
	--discovery-token-ca-cert-hash sha256:4450af2092fee1447e1e27f4fb350dc470ccd08a91c43fd61473e508bf5a6eab
