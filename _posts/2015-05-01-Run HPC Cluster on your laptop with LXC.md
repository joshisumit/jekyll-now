---
layout: post
title: How I created HPC Cluster with LXC on my laptop
---

In this post I will talk about, rapid deployment of MPI Cluster on Linux Container.It allows the cluster administrator to focus on the core cluster tasks,rather than spending time on installing linux cluster. It removes the complexity of the cluster installation and expansion process.

Note:
If you want to know [Why LXC for running HPC](/why_lxc_for_hpc) 

##Prerequisites
In this tutorial, we will be using lxc for running containers and libvirt API(virsh) for managing those containers.
Install them with:
`apt-get install lxc`

`apt-get install libvirt-bin`



As mentioned in my previous blog [Build HPC cluster from scratch with LXC](/Build HPC cluster from scratch with LXC) ,first i have created HPC Cluster with LXC manually. Just download those Ready-made filesystems for [master node](/mpi-master-fs) and [compute node](/mpi-compute-fs).

Copy these filesystems in `/var/cache/lxc` in your Ubuntu.
`cp ~/mpi-master-fs /var/cache/lxc`

`cp ~/mpi-compute-fs /var/cache/lxc`

Once MPI cluster is installed, Filesystem will be in `/var/lib/lxc/$master_node/rootfs`

On Master Node


- NFS Server
- Passwordless SSH to all compute nodes.
- MPI installation
- cluster user for running HPC jobs.



##How I created Rapid HPC Cluster with LXC.

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

Filesystem (Initially created in /var/cache/lxc and then copied to /var/lib/lxc/)

- master node (/var/cache/lxc/mpi-master/rootfs)
- compute node (/var/cache/lxc/mpi-worker/rootfs)





LXC Template

- master node (mpi-master)-(stored in /usr/lib/lxc/templates)
- compute node (mpi-worker)
- some other one



FS path = /var/lib/lxc/$master_node/rootfs



