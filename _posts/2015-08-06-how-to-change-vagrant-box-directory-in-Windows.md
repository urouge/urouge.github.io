---
layout: post
title: Windows下修改 Vagrant Box 加载目录
tag: vagrant
---
在Vagrant中添加box时，加载目录默认在 <em>~/.vagrant.d/</em>，具体的目录结构是<em>C:\Users\Your Username\\.vagrant.d\</em>,由于是在C盘中，在一些配置不高的机子中用起Vagrant，磁盘容量会显得捉襟见肘，这时会考虑更改加载box的目录。

查阅[Vagrant官网相关环境变量](http://docs.vagrantup.com/v2/other/environmental-variables.html)后发现相关的环境变量是**VAGRANT_HOME**，在Windows下设置环境变量并不是很熟悉，Google了一下在[这里](https://harvsworld.com/2014/change-vagrant_home-directory-windows/)找到了方法。

##永久设置环境变量

* 永久设置**用户**的环境变量

<pre><code class="highliter">
setx VAGRANT_HOME "/your/path"
</code></pre>

* 永久设置**系统**的环境变量

<pre><code class="highliter">
setx VAGRANT_HOME "/your/path" /M
</code></pre>

##临时设置环境变量
这样的方式只是临时性地将环境变量写入session,每次要打开新终端执行vagrant相关命令如重启关闭销毁等命令之前都需要敲设置一遍

<pre><code class="highliter">
set VAGRANT_HOME="/your/path"
</code></pre>

对于以上设置的环境变量可以在注册表中查看和修改，在注册表中相应的结构树是<em>HKEY_CURRENT_USER/Environment</em>

按照上面的方法就可以修改Vagrant的加载目录了，此外，如果C盘容量还是不够用的话可以考虑修改新建虚拟机文件的默认生成路径，我用的是 VirtualBox，具体操作是打开VirtualBox，在菜单中选择<em>管理</em>-><em>全局设定</em>(或者直接按<em>Ctrl+G</em>)打开面板，修改**默认虚拟电脑位置**的路径即可，指的一提的是，上述提到的虚拟机生成路径在新建虚拟机的过程中是可以指定的。
