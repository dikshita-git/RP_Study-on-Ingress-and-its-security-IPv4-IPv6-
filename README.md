## Securing Ingress in dual-stack K3s
This is an academic Research project carried out for intensive learning of Ingress across different platforms and determines the possible measures to secure it both using IPv4 and IPv6.

### What is K3s?:
-This is a lightweight distribution of Kubernetes where we can easily create clusters and manage it using the tool K3d which basically means K3s with Docker.
- Applicable for :
    a) IoT
    b) Edge Computing
    c) ARM etc. where the device components are smaller and holds minimal memory.
- It supports dual-stack from v1.21+
- This means both IPv4 and IPv6 works with K3S:

### K3s Dual-stack:

Steps:
1. Configure the proxmox's netplan (/etc/netplan) with the configuration as below:

<img src="">

 These settings can also be applied during the proxmox system installation.
2. Install k3s using : export INSTALL_K3S_VERSION=v1.21.3+k3s1
