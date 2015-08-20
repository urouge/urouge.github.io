---
layout: post
title: Using Nginx
description: 
tag: nginx
---

以下是一些学习使用**Nginx**的一些心得

##安装 Nginx

* ###必需工具和库
安装和使用Nginx必需的工具和库有**GCC**,**PCRE**,**zlib**,**OpenSSL**，下面分别列出Red-Hat系和Debian系的Linux系统安装方法。

  * **GCC** <br />Using yum:<pre><code class="highliter">
  yum groupinstall "Development Tools"
</code>Using apt-get:</pre><pre><code class="highliter">
  apt-get install build-essentials
</code></pre>
