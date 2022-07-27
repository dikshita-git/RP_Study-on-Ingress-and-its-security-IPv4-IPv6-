


1. /usr/local/bin/k3s-uninstall.sh

2. Install k3s + Config For IPv4 address only:
*************************

curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE="644" INSTALL_K3S_EXEC="--flannel-backend=none --cluster-cidr=192.168.0.0/16 --service-cidr=10.43.0.0/16 --disable-network-policy  --disable=traefik" sh -

3. kubectl create -f https://projectcalico.docs.tigera.io/manifests/tigera-operator.yaml


4. curl https://projectcalico.docs.tigera.io/manifests/custom-resources.yaml -O

5. kubectl create -f custom-resources.yaml

6. k3s kubectl get pods
7. cd /etc/cni/net.d/calico-kubeconfig
8. k3s kubectl get pod --all-ns
9. kubectl version -o yaml




10. Install k3s + Config For IPv4,IPv6 address :
**************************************

curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE="644" INSTALL_K3S_EXEC="--flannel-backend=none --cluster-cidr=192.168.0.0/16,2001::00/68 --service-cidr=10.43.0.0/16 --disable-network-policy --node-ip=2001:638:408:200::100:180 --disable=traefik" sh -

