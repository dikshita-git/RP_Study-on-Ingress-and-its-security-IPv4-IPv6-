## Details on my K3s Cluster

This folder consists of the normal way to create a 3-Node K3s cluster. Thanks and credits to the reference links: 

1. <a href="https://rancher.com/docs/k3s/latest/en/">K3s Official</a>

2. <a href="https://pet2cattle.com/2021/04/k3s-join-nodes">Pet2cattle</a>

In this case, I have set up a K3s cluster with:
    
              1 Master node
              **************
                      Name: master
                      Ip address: 10.20.116.104/24
              
              2 Worker nodes
              **************
                      Name: worker1
                      Ip address: 10.20.116.160/24
                      
                      Name: worker2
                      Ip address: 10.20.116.213/24
                      


### Setting up a K3s cluster in the basic way
----------------------------------------------

Setting up a K3s cluster in the most basic way with minmum nodes associated  is pretty easy with a little bit of careful configurations. but for High-availabilty, automatising K3s with ansible is highly recommended.

In order to set up the K3s cluster, below steps are followed:

#### STEPS:


#### 1. Install K3s binary in the ***master node*** with the command:  :tada:

              curl -sfL https://get.k3s.io | sh -
              
 This command is carried out in the server/master node. This installation is for a new node (server, worker any) to make it join to an existing cluster.


 #### 2. Copy the K3s node-token  :tada:
 
 To install a worker node we will have to set some environment variables for telling the installer for join an existing cluster. We will need:

                   a. Token
                   b. Master's IP
                   
The token we can get it from the master node on the following location ***/var/lib/rancher/k3s/server/node-token***. As we alredy know the IP address of the master node, so in ***K3S_URL*** we will have to specify the HTTPS protocol of our master node and specify 6443 as the port number. Also, paste teh K3s_Token that we copied from teh location /var/lib/rancher/k3s/server/node-token.

This is done by the command:
                
                curl -sfL https://get.k3s.io | K3S_URL=https://10.20.116.104:6443 K3S_TOKEN="K1022823f9c9804022be8bf04846f5da7bbcb7763063eee43290acdbc7ec66f6eb3::server:54956246f32a0d4f1cb6d4c9fb3b3f78" sh -
                

K3s server always requires the port 6443 to be accessible by all teh nodes.

But <u>***WHY?***</u> :fire:
-----------------------------

By default, the Kubernetes API server listens on port 6443 on the first non-localhost network interface, protected by TLS. Traffic on port 6443 is for the Kubernetes API. ... for securely routing traffic from a remote shell outside the cluster to the UCP ports of the cluster.
 
 
