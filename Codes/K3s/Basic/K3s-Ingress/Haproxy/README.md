## K3s Ingress in Haproxy

Here we will set up a K3s cluster using ansible with MetalLB, HAProxy, Prometheus, and a test echo server. For this, I have chosen a VM which will be my deployment environemnt for Ansible which will basically bootstrap my k3s cluster.
Thus, I have installed Ansible 2.13.2 on my deployment environemnt VM named ***depenv*** with the IP address: 10.20.116.88/24. I have also cloned the official k3s-in-ansible <a href="https://github.com/k3s-io/k3s-ansible">git repo </a> by Rancher.

Next, it is time to configure the ***inventory/hosts.ini***. here is the link to my <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Codes/K3s/Basic/K3s-Ingress/Haproxy/inventory/sample/hosts.ini">hosts.ini</a> configuration file. Once done, don't forget to activate the ***inventory*** line in <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Codes/K3s/Basic/K3s-Ingress/Haproxy/ansible.cfg">ansible.cfg</a>.

The above step is followed by copying teh ssh-key of the deployment VM to the master and worker nodes because Ansible works via ssh and passwordless access. Thus our cluster nodes can be accessible by Ansible and we get a successful ping in ansible. 

In the next step, we edit the ***/etc/ansible/hosts*** as given below::

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/10.PNG">

