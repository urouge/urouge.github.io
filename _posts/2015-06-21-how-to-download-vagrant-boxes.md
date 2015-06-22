---
layout: post
title: 如何下载Vagrant的Box
description: 
tag: vagrant
---
使用 [Vagrant](https://www.vagrantup.com/) 来管理虚拟机有相见恨晚的感觉，在安装自己喜欢的 box 的过程中相信大家都会对其缓慢的下载速度有很深刻的印象，掉过坑的人在下次安装的时候会选择先将box用迅雷或者其他方式下载到本地，再进行安装，这样的方式可以剩下大量的时间，在这里主要介绍如何下载box文件。

##终端获取下载链接

我们通常是在终端中使用如下命令生成Vagrantfile和添加box名，这里用的是<em>laravel/homestead</em>，一个PHP框架推荐使用的环境，Linux 发行版是Ubuntu 14.04，里面安装了PHP 5.6,Nginx,MySQL,Node,Redis等
<pre><code class="highlighter">
  vagrant init laravel/homestead
  vagrant up
</code></pre>
屏幕中会显示类似下面的信息
<pre><code class="highlighter">
	Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'laravel/homestead' could not be found. Attempting to find and
install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
==> default: Loading metadata for box 'laravel/homestead'
    default: URL: https://atlas.hashicorp.com/laravel/homestead
==> default: Adding box 'laravel/homestead' (v0.2.6) for provider: virtualbox
    default: Downloading: https://atlas.hashicorp.com/laravel/boxes/homestead/versions/0.2.6/providers/virtualbox.box
    default: Progress: 0% (Rate: 11710/s, Estimated time remaining: 4:33:50)==>
default: Waiting for cleanup before exiting...
</code></pre>
阅读以上信息会很容易得到box的下载地址为
`https://atlas.hashicorp.com/laravel/boxes/homestead/versions/0.2.6/providers/virtualbox.box`
用迅雷或者其他工具下载即可

##官网获取下载链接
在[这里](https://atlas.hashicorp.com/boxes/search)能找到官网所提供的box，点击进入相应的box中，里面会显示版本号和支持的虚拟机(virtualBox或者vmware_desktop)，确认自己想要的版本号和虚拟机后，点击右上角的版本号进入新页面，在当前地址后添加<em>/providers/virtualbox.box</em>可获得virtualbox版本的box下载地址，vmware_desktop版同理。
<center><img src="{{ site.url }}/images/laravel_homestead.png" alt="re"></center>
同样以[homestead](https://atlas.hashicorp.com/laravel/boxes/homestead)为例<br>

1. 进入官网上该项目的地址,`https://atlas.hashicorp.com/laravel/boxes/homestead/`
2. 点击右上角自己需要的版本，这里是<em>v0.2.6</em>
3. 进入页面后在当前页地址栏后添加providers的选择即可，最终的链接如<br>
`https://atlas.hashicorp.com/laravel/boxes/homestead/versions/0.2.6/providers/virtualbox.box`<br>

这两个方法获取的结果是一样的，第二种方法会比较方便，个人暂时没发现官网有直接提供各个版本的box下载列表，如果有更好的方法获取vagrant的box下载地址，欢迎交流指导。
