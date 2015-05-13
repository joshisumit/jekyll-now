---
layout: post
title: Quickly deploy virtualized HPC cluster with LXC on your laptop
---

Hello folks,

In this post I will talk about rapid deployment of HPC Cluster in Ubuntu 12.04.
I have written shell script, that will let you turn your laptop into a virtual three-node HPC cluster that can be used to develop and run HPC applications, including MPI apps. 

This script is useful for basic development and testing – if all you want is to test some HPC code than you can check it by running this script which rapidly deploys HPC Cluster.Virtual HPC cluster is deployed with Linux Container(LXC) and libvirt.

##Prerequisites
In this tutorial, we will be using LXC for creating filesystem of container and libvirt API(virsh) for managing those containers.Before running a script install them with:

    apt-get install lxc
    apt-get install libvirt-bin

##What we wiil do ?

We will be creating one master node and then 3 worker nodes to actually do the work.
Everthing will be installed automatically by script.

- Master Node:
  - Hostname : master
  - IP : 192.168.122.151 (IP may vary in your case, given by dnsmasq service)

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

This script requires following 5 components :

1. base.xml file
2. Master node filesystem(hpc_master_fs.tar.bz2 file)
3. Compute node filesystem(hpc_worker_fs.tar.bz2 file)
4. LXC template for master node(lxc-mpi-master file)
5. LXC template for compute node(lxc-mpi-worker file)

Download the above stuff from [here](https://drive.google.com/folderview?id=0B6LOfkglrOq9fnJ3dkpINUFOaTYySVRHOHhYMUFBcDVfcHlWMWZiUzRBbVh3ZkVtU3VmUzA&usp=sharing) .

Now extract the master and compue node filesystem:

    tar -xvjf hpc_master_fs.tar.bz2 -C /var/cache/lxc
    tar -xvjf hpc_worker_fs.tar.bz2 -C /var/cache/lxc
    
Copy LXC templates in `/usr/lib/lxc/templates` :
    
    cp lxc-mpi-master /usr/lib/lxc/templates
    cp lxc-mpi-worker /usr/lib/lxc/templates
    
Once everything is settled up,run the script:

    ./create-hpc-with-lxc-on-laptop.sh
    
Script will take some time to install master and compute node.


##Verify your HPC Cluster Installation


    virsh -c lxc:/// list
    
You will get following output:

    Id    Name                           State
    ----------------------------------------------------
    5164  master                         running
    5166  compute-1                      running
    5169  compute-2                      running
    5199  compute-3                      running
 
 
###Verify Master container  
Login to your master node container with username 'ubuntu' and password 'ubuntu' :

    virsh -c lxc:/// console master

After logging in, Check following things in your master container:

1. `cluster` user: to operate our cluster.
2. NFS Server: to share cluster user's home directory with all compute nodes.Just check it by doing:

        cat /etc/exports
        
        /home/cluster *(rw,sync,no_subtree_check)
3. `/etc/hosts` file: entry for all compute nodes.
4. Passwordless SSH: cluster user will do password less SSH login in all compute node.
5. MPI (`mpich2`):MPI is installed, cluster will communicate with MPI.


###Verify compute container 
Login to your compute-1 node container with username 'ubuntu' and password 'ubuntu' :

    virsh -c lxc:/// console compute-1

After logging in, Check following things in your compute-1 container:

1. `/etc/fstab` file
2. Verify NFS share(/home/cluster) is properly mounted.If it is not mounted than mount it by running:
    mount -a
3. `/etc/hosts` file

##Summary

If you are working with HPC, having a full blown HPC Cluster on your laptop is awesome :)
