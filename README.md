## RP_Study-on-Ingress-and-its-security-IPv4-IPv6-
This is an academic Research project carried out for intensive learning of Ingress across different platforms and determines the possible measures to secure it both using IPv4 and IPv6.

### K3s:
-This is a lightweight distribution of Kubernetes and we can easily create clusters and manage it using the tool K3d which basically means K3s with Docker.
- It supports dual-stack from v1.21+

### K3s Dual-stack:

Steps:
1. Configure the proxmox's netplan (/etc/netplan) with the configuration as below:

<img src="">

 These settings can also be applied during the proxmox system installation.
2. Install k3s using : export INSTALL_K3S_VERSION=v1.21.3+k3s1
