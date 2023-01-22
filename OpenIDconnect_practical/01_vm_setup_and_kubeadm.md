
# Basics

-kubeadm
	- passes kubernetes conformance tests
	- supports bootstrap tokens
	- All of these services are deployed as pods within Kubernetes.
		-> etcd, dns, api-server, dashboard, monitoring logging, ...

-requirements
	- choose a pod network add-on
	- install container runtime
		- kubeadm tries to detect the container runtime by using a list of well known endpoints. 
		- To use different container runtime or if there are more than one installed on the provisioned node, specify the --cri-socket argument to kubeadm.


-options
	--control-plane-endpoint (recommended)
		- if you have plans to upgrade this single control-plane kubeadm cluster to high availability you should specify the --control-plane------endpoint to set the shared endpoint for all control-plane nodes
		- Such an endpoint can be either a DNS name or an IP address of a load-balancer.
	--pod-network-cidr
		- Depending on which third-party provider you choose, you might need to set the --pod-network-cidr to a provider-specific value
	--cri-socket
		- select container runtime
	--apiserver-advertise-address=<ip-address>
		- advertise address of the api server
		- by default network interface associated with the default gateway
		- ipv6 geht auch --apiserver-advertise-address=2001:db8::101


### STEPS:

#### 0. provision vms
    - via netlab, etc.
    
#### 1. install kubeadm, kubectl, containerd
    source: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
```
    $ sudo apt-get update
    $ sudo apt-get install -y apt-transport-https ca-certificates curl
    $ sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
    $ echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
    $ sudo apt-get update
    $ sudo apt-get install -y kubelet kubeadm kubectl containerd htop
```
	
#### 2. enable and start services for kubernetes
on both master and worker:
```
   	$ sudo systemctl enable containerd
	$ sudo systemctl start containerd
	$ sudo systemctl enable kubelet.service
	$ sudo systemctl start kubelet.service
```
	
#### 3. disable swap
```
	$ sudo swapoff -a # preflight prerequisite for kubeadm
	$ sudo vim /etc/fstab <- disable swap
```
				 
#### 4. install eye candy and aliases
```
    $ sudo apt-get install grc # generic colourizer
```
				 
- append to our ~/.bashrc (for both root and unpriviledged users):
on both nodes

- (we need to change user to your username)
				 
```
$ export user=worker2                             
$ cat << EOF >> /root/.bashrc
GRC_ALIASES=true
[[ -s "/etc/profile.d/grc.sh" ]] && source /etc/profile.d/grc.sh
EOF
```

```
echo "alias k='grc kubectl'" >> /root/.bashrc

$ sudo ln -s /usr/bin/vim /usr/bin/vi

$ cat << EOF >> /home/${user}/.bashrc
GRC_ALIASES=true
[[ -s "/etc/profile.d/grc.sh" ]] && source /etc/profile.d/grc.sh
EOF
echo "alias k='grc kubectl'" >> /home/${user}/.bashrc

if the file does not exist:
$ wget https://raw.githubusercontent.com/garabik/grc/master/grc.sh -O /etc/profile.d/grc.sh
    
$ exit # open new terminal to apply changes
$ ssh again

```
	
#### 5. kubernetes prerequisites
needed kernel params and modules https://kubernetes.io/docs/setup/production-environment/container-runtimes/
    
##### load kernel modules on boot:
```
$ cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
```
	
##### load kernel modules right now without reboot:
```
$ sudo modprobe overlay
$ sudo modprobe br_netfilter

#set kernel params on boot:
$ cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF
```
	
##### apply kernel params right now without reboot:
```
$ sudo sysctl --system
```
	
##### verify kernel modules are present:
```
lsmod | grep br_netfilter
lsmod | grep overlay
```
	
##### verify values are set:
```
$ sysctl net.bridge.bridge-nf-call-iptables net.bridge.bridge-nf-call-ip6tables net.ipv4.ip_forward
```
	
#### 6. give crictl the correct endpoint, it can autodetect but will complain:
	
- CRI is container runtime interface, the sock file is used to speak CRI protocol with the socket
but crictl doesn't it needs to autodetect
```
$ sudo vi /etc/crictl.yaml
runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 2
debug: true
```
    
#### 7. install kubernetes cluster
	
- on master
	
	```
		sudo kubeadm init --apiserver-advertise-address=10.20.116.209 --pod-network-cidr=10.20.70.0/24
		(on problem kubeadm reset)
        ```
		(blkio error is a bug which can be ignored)
- on worker (copy paste the tokens, do not forget sudo)
	```
		sudo kubeadm join 10.20.116.209:6443 --token zuagwf.l0nucgt0y6k2uhy7 \
        --discovery-token-ca-cert-hash sha256:2c2ba1e758d89be33243e8e7da3a7238af5976b046e9dcd759f744da15a7fee5 
        ```
- on master
		set up kubectl (commands are recommended by kubeadm itself)
      ```
	mkdir -p $HOME/.kube
  	  sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
  	  sudo chown $(id -u):$(id -g) $HOME/.kube/config
      ```
- verify that it worked
	
		▶️kubectl get node 
	
		- we miss an advanced networking plugin

#### 8. install networking plugin calico

- wget https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/tigera-operator.yaml
- kubectl create -f tigera-operator.yaml
- wget https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/custom-resources.yaml
- vim custom-resources.yaml
    - change pod cidr to one not interfering with the nodes LAN:
        
	10.20.70.0/24

    - if we have multiple interfaces, by default the first one is selected
    - we need to specify another interface if the first one is not the interface over which our cluster runs
       ``` 
	spec:
          calicoNetwork:
            nodeAddressAutodetectionV4:
              interface: "eth1"
        ```
	
    - kubectl create -f custom-resources.yaml

- verify all the calico pods run
    
	▶️ k get -n calico-system all
    
- verify that nodes have correct ip autodetected
   
	▶️ k get nodes -o wide
    
- (optional debug cmds) calico self-recognized ip
    
	▶️ k get -n calico-system all
    
	▶️ k describe -n calico-system pod/calico-node-5bmmj
    
	▶️k describe -n calico-system daemonset.apps/calico-node


----------------------------------------------------

🔴 Encountered problems:

- needed to make containerd work with kubernetes, mismatching cgroup versions:
		
	- randomly shutting down pods 
				https://www.jetstack.io/blog/cri-migration/
	
				https://gjhenrique.com/cgroups-k8s/
	
				https://github.com/etcd-io/etcd/issues/13670
	
			🟢 Solution:
	           
	           Edit on both nodes:
	        ```
		
	        sudo mkdir /etc/containerd
	
		sudo vim /etc/containerd/config.toml
	
		version = 2
	
		[plugins]
	
		  [plugins."io.containerd.grpc.v1.cri"]
	
		   [plugins."io.containerd.grpc.v1.cri".containerd]
	
		      [plugins."io.containerd.grpc.v1.cri".containerd.runtimes]
	
		        [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
	
		          runtime_type = "io.containerd.runc.v2"
	
		          [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
	
		            SystemdCgroup = true

	        ```
	
	- sudo systemctl restart containerd

- the self detected ip of nodes (calico) and of the kubelet might be wrong
    
	-> calico can be fixed by changing custom-resources.yaml
    
	-> kubelet can be fixed like this:
	
	```
		sudo vim /etc/kubernetes/kubelet.env
		  --node-ip=192.168.56.2
	
		  and
	
		  --node-ip=192.168.56.3
	
		 sudo systemctl daemon-reload
	
		 sudo systemctl restart kubelet
	 ```
       -> verify via INTERNAL-IP bei cmd
		 
	      ▶️k get nodes -o wide
