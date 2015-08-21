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

  * **PCRE**(Perl Compatible Regular Expression) <br />Using yum:<pre><code class="highliter">
  yum install pcre pcre-devel
</code>Using apt-get:</pre><pre><code class="highliter">
  apt-get install libpcre3 libpcre3-dev
</code></pre>

  * **zlib** <br />Using yum:<pre><code class="highliter">
  yum install zlib zlib-devel
</code>Using apt-get:</pre><pre><code class="highliter">
  apt-get install zlib1g zlib1g-dev
</code></pre>

  * **OpenSSL** <br />Using yum:<pre><code class="highliter">
  yum install openssl openssl-devel
</code>Using apt-get:</pre><pre><code class="highliter">
  apt-get install openssl openssl-dev
</code></pre>

* ###下载和编译Nginx
在[官网](http://nginx.org/)下载所需版本，解压，编译，安装，截止至本文完成，最新的Nginx稳定版为**nginx-1.8.0**<pre><code class="highliter">
  wget http://nginx.org/download/nginx-1.8.0.tar.gz
  tar zxvf nginx-1.8.0.tar.gz
  cd nginx-1.8.0
  ./configure
  make && make install
</code></pre>
在编译前可以有很多配置选项供选择，如指定安装路径，sbin路径，配置文件路径，pid路径，编译的模块等，按需选择，上面的编译过程选择的是默认的安装，默认的安装路径是<em>/usr/local/nginx</em>，以下提供了所有模块的编译选项：<pre><code class="highliter">
  ./configure --user=www-data --group=www-data --with-http_ssl_module --with-http_realip_module --with-http_addition_module --with-http_xslt_module --with-http_image_filter_module --with-http_geoip_module --with- http_sub_module --with-http_dav_module --with-http_flv_module --with- http_mp4_module --with-http_gzip_static_module --with-http_random_index_module --with-http_secure_link_module --with-http_stub_status_module --with-http_perl_module --with-http_degradation_module
</code></pre>
值得注意的是，编译模块指定路径，需要保证所需类库的目录存在以及可写，安装完成后，进入<em>/usr/local/nginx/sbin</em>目录，执行<em>./nginx</em>命令就可以启动Nginx了，如果提示80端口被占用，执行 netstat -antp 查看占用端口的进程，kill 掉即可。

##常用 Nginx 命令行
根据自己的Nginx安装目录，以下命令是在本文安装目录<em>/usr/local/nginx/sbin</em>中执行。

* **nginx -s stop**     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;强制关闭Nginx守护进程，等同于**kill TERM pid** 或者 **kill INT pid**
* **nginx -s quit**     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;优雅地关闭守护进程，等同于**kill QUIT pid**，服务器处理完请求后关闭进程
* **nginx -s reload**   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;重新加载配置文件，等同于**kill HUP pid**，打开一个新的worker进程，旧进程处理完请求后被关闭
* **nginx -s reopen**   &nbsp;&nbsp;&nbsp;&nbsp;重读日志文件，等同于**kill USR1 pid**



