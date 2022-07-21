- #### k3d cluster create "Name_of_cluster"

      = This command will create a cluster of specified name along with a loadbalancer. If we dont specify the cluster name, k3d will name it as k3s-default-cluster.

- #### k3d cluster list

      = List aall the clusters
      
- #### k3d node create "Worker-Node-name" --replicas 2 --cluster "cluster_name_to_which_you_want_to_add_workers"
      
      = This command basically creates 2 Worker nodes with the specified name(01,02 will be allocated to them by k3d itself) inside the  specified cluater name.
      
- #### k3d cluster delete -a 
 
      =  Deletes all clusters 
     
- #### k3d cluster delete "name_of_cluster" 

      = Deletes specified cluster 
