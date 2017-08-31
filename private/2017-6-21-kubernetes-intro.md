---
layout: page
sitemap: false
noindex: true
title: Kubernetes 
categories: docker kubernetes container-orchestration
---

... some description ...

## Masters - The Kubernetes Control Plane

 - **apiserver** (_kube-apiserver_)
    - Front-end to the control plane
    - Exposes the API (REST)
    - Consumes JSON (via manifest files)
 - **Cluster Store**
    - Persistent storage
    - Cluster state and config
    - Uses **etcd**
        - Distributed, consistent, watchable… The “source of truth” for the cluster
        - Have a backup plan for it!
 - **kube-controller-manager**
    - Controller of controllers
        - Node controller
        - Endpoints controller
        - Namespace controller
        - …
    - Watches for changes
    - Helps maintain desired state
 - **kube-scheduler**
    - Watches apiserver for new pods
    - Assigns work to nodes
        - affinity/anti-affinity
        - constraints
        - resources
        - …

## Node (minions)
 - **Kubelet**
    - The main Kubernetes agent
    - Registers node with cluster
    - Watches apiserver
    - Instantiates pods
    - Reports back to master
    - Exposes endpoint on :10255
 - **Container Engine** 
    - Does container management:
        - Pulling images
        - Starting/stopping containers
    - Pluggable: **Usually is Docker**, but can be rkt
 - **kube-proxy**
    - Kubernetes networking:
        - Pod IP addresses
        - All containers in a pod share a single IP
        - Load balances across all pods in a service


## Abstractions

### Pod
 - Containers always run inside of pods
 - Pods can have multiple containers (advanced use-case)
    - Ring-fenced environment
        - Network stack
        - Kernel namespaces
        - All containers in pod share the pod environment - Tight Coupling
    - The unit of scale of Kubernetes is a Pod (not a container)
