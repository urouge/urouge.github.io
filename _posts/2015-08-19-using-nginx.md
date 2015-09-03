---
layout: post
title: Using Nginx
description: 以下是一些学习使用Nginx的一些心得，相关笔记大多来自PACKT的《Nginx HTTP Server, 2nd Edition》
tag: nginx
---

以下是一些学习使用**Nginx**的一些心得，相关笔记大多来自**PACKT**的《**Nginx HTTP Server, 2nd Edition**》

##安装 Nginx

* ###必需工具和库
安装和使用Nginx必需的工具和库有**GCC**,**PCRE**,**zlib**,**OpenSSL**，下面分别列出Red-Hat系和Debian系的Linux系统安装方法。

  * **GCC** <br />Using yum:<pre><code class="highlighter">
  yum groupinstall "Development Tools"
</code>Using apt-get:</pre><pre><code class="highlighter">
  apt-get install build-essentials
</code></pre>

  * **PCRE**(Perl Compatible Regular Expression) <br />Using yum:<pre><code class="highlighter">
  yum install pcre pcre-devel
</code>Using apt-get:</pre><pre><code class="highlighter">
  apt-get install libpcre3 libpcre3-dev
</code></pre>

  * **zlib** <br />Using yum:<pre><code class="highlighter">
  yum install zlib zlib-devel
</code>Using apt-get:</pre><pre><code class="highlighter">
  apt-get install zlib1g zlib1g-dev
</code></pre>

  * **OpenSSL** <br />Using yum:<pre><code class="highlighter">
  yum install openssl openssl-devel
</code>Using apt-get:</pre><pre><code class="highlighter">
  apt-get install openssl openssl-dev
</code></pre>

* ###下载和编译Nginx
在[官网](http://nginx.org/)下载所需版本，解压，编译，安装，截止至本文完成，最新的Nginx稳定版为**nginx-1.8.0**<pre><code class="highlighter">
  wget http://nginx.org/download/nginx-1.8.0.tar.gz
  tar zxvf nginx-1.8.0.tar.gz
  cd nginx-1.8.0
  ./configure
  make && make install
</code></pre>
在编译前可以有很多配置选项供选择，如指定安装路径，sbin路径，配置文件路径，pid路径，编译的模块等，按需选择，上面的编译过程选择的是默认的安装，默认的安装路径是<em>/usr/local/nginx</em>，以下提供了所有模块的编译选项：<pre><code class="highlighter">
  ./configure --user=www-data --group=www-data --with-http_ssl_module --with-http_realip_module --with-http_addition_module --with-http_xslt_module --with-http_image_filter_module --with-http_geoip_module --with- http_sub_module --with-http_dav_module --with-http_flv_module --with- http_mp4_module --with-http_gzip_static_module --with-http_random_index_module --with-http_secure_link_module --with-http_stub_status_module --with-http_perl_module --with-http_degradation_module
</code></pre>
值得注意的是，编译模块指定路径，需要保证所需类库的目录存在以及可写，安装完成后，进入<em>/usr/local/nginx/sbin</em>目录，执行<em>./nginx</em>命令就可以启动Nginx了，如果提示80端口被占用，执行 <em>netstat -antp</em> 查看占用端口的进程，kill 掉即可。

##常用 Nginx 命令行
根据自己的Nginx安装目录，以下命令是在本文安装目录<em>/usr/local/nginx/sbin</em>中执行。

* **nginx -s stop**     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;强制关闭Nginx守护进程，等同于**kill TERM pid** 或者 **kill INT pid**
* **nginx -s quit**     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;优雅地关闭守护进程，等同于**kill QUIT pid**，服务器处理完请求后关闭进程
* **nginx -s reload**   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;重新加载配置文件，等同于**kill HUP pid**，打开一个新的worker进程，旧进程处理完请求后被关闭
* **nginx -s reopen**   &nbsp;&nbsp;&nbsp;&nbsp;重读日志文件，等同于**kill USR1 pid**

上面的 **pid** 是Nginx的主进程号，可以通过 **ps aux |grep nginx **查看 master process 进程或者查看Nginx安装目录下的<em>logs</em>目录下的nginx.pid文件可以获得。也可以在命令行中用 **cat /usr/local/nginx/logs/nginx.pid **代替。

##测试配置文件

在修改完配置文件后，如需检测配置文件的语法是否正确，可以通过以下命令进行检测：
<pre><code class="highlighter">
  /usr/local/nginx/sbin/nginx –t
</code></pre>
配置文件语法通过则提示成功，若有错误则会提示在哪个地方出现错误。值得注意的是，在线上环境直接测试正在使用的配置文件是不恰当的，这种情况通过 -c 参数测试另外一个配置文件，命令如下:
<pre><code class="highlighter">
  /usr/local/nginx/sbin/nginx -t -c /your/testconf/path/nginx.conf
</code></pre>
测试确认通过后可执行完成线上环境的配置文件修改。
<pre><code class="highlighter">
  cp -i /your/testconf/path/nginx.conf /usr/local/nginx/conf/nginx.conf

  /usr/local/nginx/sbin/nginx -s reload
</code></pre>

##平滑升级 Nginx
在升级Nginx过程中经常会有这样的操作，关闭服务器，替换sbin目录下的二进制文件，重新开启服务器，这对于业务不多的网站来说是可行的，但是对于大网站来说会导致大量的连接丢失和业务中断，Nginx提供了一个机制允许在替换二进制文件的过程中保证不丢失连接。
在上面提到过kill 命令后加上信号量和pid可对 Nginx 进行控制，这里需要**USR2**和**WINCH**这两个信号量，具体过程如下：

  1. 用新的二进制文件替换安装目录下的文件<em>/usr/local/nginx/sbin/nginx</em>
  2. 执行命令**cat /usr/local/nginx/logs/nginx.pid**查看pid进程号
  3. 执行命令**kill USR2 pid**，初始化升级，保持旧的 .pid 文件和启用新的nginx二进制文件
  4. 执行命令**kill WINCH pid**，关闭旧的 worker progresses
  5. 确保所有的 worker progresses 都关闭之后，执行命令**kill QUIT pid**

至此，Nginx已经平滑地升级成功。