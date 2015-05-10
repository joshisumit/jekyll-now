---
layout: post
title: How I created HPC Cluster with LXC on my laptop
---



HPC Cluster Components

- No problems exist
- You made it all up



On Master Node
- NFS Server
- Passwordless SSH to all compute nodes.
- MPI installation



How I created HPC Cluster with LXC.

We will be creating one control node and then 3 worker nodes to actually do the work.

Base OS: Ubuntu 12.04

Filesystem of
- master node
- compute node





LXC Template
- master node (mpi-master)-(stored in /usr/lib/lxc/templates)
- compute node (mpi-worker)
- some other one



FS path = /var/lib/lxc/$master_node/rootfs




- No problems exist
- You made it all up
 1. You probably missing something big
     - That isn't really important though
     - I don't mean to pry
 2. You might have just not indented enough times
     - That seems more likely
- This helps, right?

