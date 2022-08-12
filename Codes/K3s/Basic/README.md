## Details on my K3s Cluster

This folder consists of the normal way to create a 3-Node K3s cluster.

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

Setting up a K3s cluster in the most basic way with minmum nodes associated  is pretty easy with a little bit of careful configurations. but for High-availabilty, automatising K3s with ansible is highly recommended.

In order to set up the K3s cluster, below steps are followed:

:tada: Install K3s binary in the ***master node*** with the command:

              curl -sfL https://get.k3s.io | sh -
 This installation is carried out in the server/master node as it is the control plane and the worker nodes will be associated to it using teh k3s_server_token that we will receive after the installtion in the directory ***/var/lib/rancher/k3s/server/node-token***
