# Automatised K3s with Ansible

Setting up K3s cluster manually can be very tiresome task to do specially with High-availability. Thus uding automation tool like Ansible help us carry out the tasks efficiently. Once our VMs are configured with the OS, we are good to get started. Whilst there are some steps to be followed in order to craete the k3s cluster using Ansible, here are they listed:



## Step 1: Install Ansible

Ansible should be installed in one of our machines which is playing the role of a deployment environment (not any master or server node). For installing Ansible in ubuntu, follow this <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Installation/Ansible">guide</a>

In my case, I have:

      Master nodes: 2
      *******************
      
          1. master1: IP address    -------> 10.20.116.104/24
          
          2. master2: IP address    -------> 10.20.116.209/24
      
      
      
      Worker nodes: 2
      *******************
      
          1. worker1: IP address    -------> 10.20.116.160/24
          
          2. worker2: IP address    -------> 10.20.116.213/24


      
      Deployment environment: 1
      **********************
      
             depenv: IP address    ------->  10.20.116.88/24
             



## Step 2: Git clone

In the deployment environment machine, clone the official k3s+ansible <a href="https://github.com/k3s-io/k3s-ansible.git">Git repo</a>

Once the repo is cloned, we now have to configure the inventory file according to our requirements. Instead of applying the configurations in the default inventory file, we create its copy and edit there.

      
