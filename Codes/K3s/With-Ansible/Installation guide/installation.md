## Introduction

Ansible is an opn-source automation and orchestration toll for software provisioning, configuration management and software deployment. It runs using SSH, thus also provides a layer of security in the operations to be carried out.

## Why K3s with Ansible?

Creating a Kubernetes cluster manually becomes hectic when we have multiple nodes/servers associated. By installing K3s using Ansible which is an officially avalaible installation git repo by Rancher, we simplify creation of K3s cluster. 

## Requirements

1. Ansible installed in server/deployment node
2. SSH access without password in all the servers, worker nodes
