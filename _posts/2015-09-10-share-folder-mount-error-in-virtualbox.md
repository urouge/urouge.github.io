---
layout: post
title: VirtualBox下挂载共享文件目录问题处理
description: 使用Vagrant运行一个6.5版本的**CentOS**系统一直正常运行着，直到今天早上用**vagrant up**正常启动后，出现如下提示：
tag: vagrant,linux,centos,virtualbox
---

使用Vagrant运行一个6.5版本的**CentOS**系统一直正常运行着，直到今天早上用**vagrant up**正常启动后，出现如下提示：
<pre><code class="highlighter">
Failed to mount folders in Linux guest. This is usually because
the "vboxsf" file system is not available. Please verify that
the guest additions are properly installed in the guest and
can work properly. The command attempted was:

mount -t vboxsf -o uid=`id -u vagrant`,gid=`getent group vagrant | cut -d: -f3`
vagrant /vagrant
mount -t vboxsf -o uid=`id -u vagrant`,gid=`id -g vagrant` vagrant /vagrant

/sbin/mount.vboxsf: mounting failed with the error: No such device
</code></pre>

系统是可以ssh过去的，唯一的问题只是如提示所说的挂载共享文件夹出现错误，除了共享文件夹不能使用外，带来的另一个问题是在启动系统的过程中需要消耗大量的时间，这是必须要解决的，于是我进入系统后尝试手动挂载，但是出现的如下提示：
<pre><code class="highlighter">
# mount -t vboxsf vagrant /vagrant
/sbin/mount.vboxsf: mounting failed with the error: No such device
</code></pre>

经过一系列Google和StackOverflow了解到这不是Vagrant的bug，而是VirtualBox的问题，尝试过一些网友提供的方式并不奏效，最后找到如下指令解决问题:
<pre><code class="highlighter">
cd /opt/VBoxGuestAdditions-*/init  
sudo ./vboxadd setup
</code></pre>
