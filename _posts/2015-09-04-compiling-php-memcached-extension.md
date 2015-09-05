---
layout: post
title: 编译PHP Memcached 扩展
description: 在Linux下编译PHP的Memcached扩展的时候遇到一些问题，在这里总结一下解决的过程。
tag: php,pecl,memcached
---

在Linux下编译PHP的Memcached扩展的时候遇到一些问题，在这里总结一下解决的过程。

一开始尝试用**PECL**快速安装扩展，在编译的过程中提示需要**libmemcached**依赖库，于是用yum安装了**libmemcached**和**libmemcached-devel**，但是还是没安装成功，去PHP官网的<a href="http://php.net/manual/zh/memcached.requirements.php" target="_blannk">Memcached扩展需求页</a>看了一下，**libmemcached**依赖库的版本需要大于等于1.0.0，查了一下本机该库的版本得到如下提示：
<pre><code class="highlighter">
yum list installed |grep libmemcached
libmemcached.i686                    0.31-1.1.el6              @base
libmemcached-devel.i686              0.31-1.1.el6              @base
</code></pre>
**libmemcached** 版本是0.31-1.1，并且提示已经是最新了，并不太明白这一点。于是按照官网的提示在<a href="http://libmemcached.org/libMemcached.html" target="_blannk">这里</a>下载了最新的版本1.0.18并在本地安装编译，指定路径在<em>/usr/local/libmemcached</em>

重新使用**PECL**安装并制定**libmemcached**路径后会出现如下提示：
<pre><code class="highlighter">
configure: error: no, sasl.h is not available. Run configure with --disable-memcached-sasl to disable this check
</code></pre>

于是我决定手动编译了，提示很简单，加上 **--disable-memcached-sasl**即可，接下来的过程都很顺利了。

  1. 在**PECL**官网下载memcached扩展源码并解压进入生成目录
  2. 执行命令**/usr/local/php/bin/phpize**生成编译文件，PHP 路径因安装路径而异
  3. 执行命令**./configure --with-php-config=/usr/local/php/bin/php-config --with-libmemcached-dir=/usr/local/libmemcached --disable-memcached-sasl**
  4. 执行命令**make && make install** 会在PHP扩展目录生成memcached.so
  5. 在php.ini文件添加**extension=memcached.so** 即可
  
上面的步骤适用于平时我们手动安装扩展，这次安装memcached扩展遇到问题并顺利解决再次说明了官方文档是第一手最好的资料。
