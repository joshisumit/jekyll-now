---
layout: post
title: How I created HPC Cluster with LXC on my laptop
---



##How we are creating typical HPC Cluster ?

On Master Node



- NFS Server
- Passwordless SSH to all compute nodes.
- MPI installation



##How I created HPC Cluster with LXC.

We will be creating one control node and then 3 worker nodes to actually do the work.

- Master Node:
  - Hostname : master
  - IP : 10.0.0.1



- Compute Node 1:
  - Hostname : compute1
  - IP : 10.0.0.10


- Compute Node 2:
  - Hostname : compute2
  - IP : 10.0.0.11


- Compute Node 3:
  - Hostname : compute3
  - IP : 10.0.0.12


Base OS: Ubuntu 12.04

Filesystem of

- master node
- compute node





LXC Template

- master node (mpi-master)-(stored in /usr/lib/lxc/templates)
- compute node (mpi-worker)
- some other one



FS path = /var/lib/lxc/$master_node/rootfs



