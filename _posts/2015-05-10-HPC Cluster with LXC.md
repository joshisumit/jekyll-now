---
layout: post
title: HPC Cluster with LXC
---

On Master Node: 
* NFS Server 
* Passwordless SSH to all compute nodes

How I created HPC Cluster with LXC.

We will be creating one control node and then 3 worker nodes to actually do the work.

Base OS: Ubuntu 12.04

Filesystem of 
* master node 
* compute node

LXC Template 
* master node (mpi-master)-(stored in /usr/lib/lxc/templates) 
* compute node (mpi-worker)

FS path = /var/lib/lxc/$master_node/rootfs
