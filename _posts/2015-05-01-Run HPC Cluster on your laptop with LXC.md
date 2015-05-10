---
layout: post
title: How I created HPC Cluster with LXC on my laptop
---

##Create HPC Cluster

How I created HPC Cluster with LXC.

Base OS: Ubuntu 12.04

Filesystem of
- master node
- compute node

LXC Template
- master node (mpi-master)-(stored in /usr/lib/lxc/templates)
- compute node (mpi-worker)

FS path = /var/lib/lxc/$master_node/rootfs


