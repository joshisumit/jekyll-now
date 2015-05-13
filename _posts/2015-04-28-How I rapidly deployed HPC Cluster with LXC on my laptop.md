---
layout: post
title: How I rapidly deployed HPC Cluster with LXC on my laptop
---

Quickly Deploy/Run virtualized HPC cluster with LXC on your laptop

Creating a virtualized HPC cluster using Linux Containers

##Why and how I created a working HPC Cluster on my laptop

##What this post is about?
##What you are going to do?
##What users have to do?
##Explain script components

Hello folks,

In this post I will talk about rapid deployment of HPC Cluster in Ubuntu 12.04.
I have written shell script, that will let you turn your laptop into a virtual three-node HPC cluster that can be used to develop and run HPC applications, including MPI apps. 

This script is useful for basic development and testing – if all you want is to test some HPC code than you can check it by running this script which rapidly deploys HPC Cluster.Virtual HPC cluster is deployed with Linux Container(LXC) and libvirt.


I have created pre-configured containers that includes all the components you need:


##What is LXC?


##Prerequisites
In this tutorial, we will be using LXC for creating filesystem of container and libvirt API(virsh) for managing those containers.Before running a script install them with:

    apt-get install lxc
    apt-get install libvirt-bin

##What we wiil do ?

We will be creating one master node and then 3 worker nodes to actually do the work.
Everthing will be installed automatically by script.

- Master Node:
  - Hostname : master
  - IP : 192.168.122.151 (IP can change, given by dnsmasq service)

- Compute Node 1:
  - Hostname : compute1
  - IP : 192.168.122.152

- Compute Node 2:
  - Hostname : compute2
  - IP : 192.168.122.153

- Compute Node 3:
  - Hostname : compute3
  - IP : 192.168.122.154

##So, How to set it up?

Download the script by running:

    git clone https://github.com/joshisumit/HPC-lxc-laptop.git

Download required stuff from [here](https://drive.google.com/folderview?id=0B6LOfkglrOq9fnJ3dkpINUFOaTYySVRHOHhYMUFBcDVfcHlWMWZiUzRBbVh3ZkVtU3VmUzA&usp=sharing) .
    



##Filesystems for master and compute node

As mentioned in my previous blog [Build HPC cluster from scratch with LXC](/Build HPC cluster from scratch with LXC) ,I have created HPC Cluster with LXC manually. Just download those Ready-made filesystems for [master node](/mpi-master-fs) and [compute node](/mpi-compute-fs). Extract both of them in `/var/cache/lxc`.

Just check by typing:

`ls /var/cache/lxc` 

Master node filesystem path is `/var/cache/lxc/mpi-master/rootfs`

Compute node filesystem path `/var/cache/lxc/mpi-worker/rootfs`

Pre-configured master node filesystem has:

- `cluster` user: to operate our cluster.
- NFS Server: to share cluster user's home directory with all compute nodes.
Just check it by doing:

`cat /var/cache/lxc/mpi-master/rootfs/etc/exports` 

it will look like this:

`/home/cluster *(rw,sync,no_subtree_check)`

- Passwordless SSH: cluster user will do password less SSH login in all compute node.
- MPI (`mpich2`): Cluster will communicate with MPI.


Once MPI cluster is installed, Filesystem will be in `/var/lib/lxc/$master_node/rootfs`


##LXC Template for master and compute node

Typically, all LXC templates are stored in /usr/lib/lxc/templates.I have modified basic ubuntu template for creating two different templates. Just download [master node template](/mpi-master-template) and [compute node template](mpi-worker-template).

When we create containers with lxc-create, we are specifying template name and container name like:

`lxc-create -t ubuntu -n demo`

Same here for creating master node container `mpi-master` will be used:

`lxc-create -t mpi-master -n master`

For creating compute-node container `mpi-worker` will be used:

`lxc-create -t mpi-worker -n compute1`

Now, filesystem of master and compute node is created in `/var/lib/lxc`.

##virsh

Now we will manage our container with virsh. In libvirt, every VM's configuration is defined in xml file. Download this sample [`base.xml`](/base_xml) file.



`virsh -c lxc:/// define $master_name.xml`

`virsh -c lxc:/// start $master_name`

`virsh -c lxc:/// list --all`

`ip=$(tail -n15 $path/var/lib/dhcp/dhclient.eth0.leases | grep fixed-address | cut -d" " -f4 | cut -d";" -f1)`




`sed -i "s/master/$master_name/" $path/etc/fstab`

##Script

    path=/var/lib/lxc/$master_name/rootfs
    lxc-create -t mpi-master -n $master_name
    #preapre xml of container($3)
    cp base.xml $master_name.xml
    sed -i "s/NAME/$master_name/" $master_name.xml
    sed -i "s<ROOT<$path<" $master_name.xml




##Summary

If you are working with HPC, having a full blown HPC Cluster on your laptop is awesome :)
