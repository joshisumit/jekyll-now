---
layout: post
title: Do some magic with Linux Container
---

Linux Container makes your life easy if you want virtual environment (shown below).

![_config.yml]({{ site.baseurl }}/images/config.png)

The easiest way to work with Linux Container is to use libvirt.
With libvirt u can directly deal with containers by running following command:

`virsh -c lxc:/// list`

`virsh -c lxc:/// start demo`

LXC has two core features of Linux:
- namespaces
- cgroup
 



For more instructions head over to the [Linux Container Website](http://linuxcontainer.org).
