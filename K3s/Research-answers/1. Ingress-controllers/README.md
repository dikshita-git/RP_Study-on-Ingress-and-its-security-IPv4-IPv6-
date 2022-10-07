## <ins>Question:</ins> 

<i>What are the best use-cases and scenarios for using Traefik Vs HaProxy Vs Nginx Ingress Controllers and how they differ in their operations?</i>


### Introduction

In order to understand Ingress Controller and the operational differences between its types, we have to first get along with Ingress and Ingress Controllers: why do we need them and how do they align to support each other in the kubernetes cluster.

***1. Why do we need Ingress and Ingress Controllers ?***

Let us consider an application named <b>"App"</b> deployed on Kubernetes cluster with 2 number of replicas which run on 2 different Worker nodes inside the cluster as shown below:

<img src="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Wiki-page-images/Research_Question/1.%20Ingress/1.drawio.png" width=800>

