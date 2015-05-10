---
layout: post
title: How I created HPC Cluster with LXC on my laptop
---

In this post I will talk about, rapid deployment of MPI Cluster on Linux Container in Ubuntu 12.04. It allows the cluster administrator to focus on the core cluster tasks,rather than spending time on installing linux cluster. It removes the complexity of the cluster installation and expansion process.


Note:
If you want to know [Why LXC for running HPC](/why_lxc_for_hpc) 

##Prerequisites
In this tutorial, we will be using lxc for running containers and libvirt API(virsh) for managing those containers.
Install them with:

`apt-get install lxc`

`apt-get install libvirt-bin`

##What we wiil do ?

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



##Filesystems for master and compute node

As mentioned in my previous blog [Build HPC cluster from scratch with LXC](/Build HPC cluster from scratch with LXC) ,I have created HPC Cluster with LXC manually. Just download those Ready-made filesystems for [master node](/mpi-master-fs) and [compute node](/mpi-compute-fs). Extract both of them in `/var/cache/lxc`.

Just check by typing:

`ls /var/cache/lxc` 

Master node filesystem path is `/var/cache/lxc/mpi-master/rootfs`

Compute node filesystem path `/var/cache/lxc/mpi-worker/rootfs`

Pre-configured master node filesystem has:

- `cluster` user: to operate our cluster.
- NFS Server: to share cluster user's home directory with all compute nodes.
If you do `cat /var/cache/lxc/mpi-master/rootfs/etc/exports` it will look like this:

`/home/cluster *(rw,sync,no_subtree_check)`

- Passwordless SSH: cluster user will do password less SSH login in all compute node.
- MPI (`mpich2`): Cluster will communicate with MPI.


Once MPI cluster is installed, Filesystem will be in `/var/lib/lxc/$master_node/rootfs`




LXC Template

- master node (mpi-master)-(stored in /usr/lib/lxc/templates)
- compute node (mpi-worker)
