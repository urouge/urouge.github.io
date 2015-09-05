---
layout: post
title: Using PHP-FPM and PECL
description: 在这里叙述一下个人开始使用PHP时的一些常用操作，主要是编译和开启扩展相关，笔记内容大部分来源于整理官网文档。
tag: php,php-fpm,pecl,nginx
---

在这里叙述一下个人开始使用PHP时的一些常用操作，主要是编译和开启扩展相关，笔记内容大部分来源于整理官网文档。

##安装 PHP 和开启 PHP-FPM

* ###必需的库
编译安装的过程少不了开发工具，在<a href="{{ site.url }}/using-nginx" target="_blank">这篇文章</a>有开发工具安装的介绍，安装PHP和PHP-FPM还需要两个库，分别是**libevent**和**libxml**，下面分别列出Red-Hat系和Debian系的Linux系统安装方法。

  * **Using yum**:<pre><code class="highlighter">
  yum install libevent-devel libxml2-devel
</code>
  * **Using apt-get**:</pre><pre><code class="highlighter">
  aptitude install libxml2-dev libevent-dev
</code></pre>

* ###下载和编译
在<a href="http://php.net/downloads.php" target="_blank">PHP官网</a>下载所需版本，解压并进入文件目录，编译安装的方法很简单，都只是用./configure,make,make install这些命令，根据需要在configure的时候添加需要的参数即可。**PHP-FPM**(PHP-FastCGI Process Manager)是PHP一个进程管理器，以下是开启php-fpm的方法<pre><code class="highlighter">
  ./configure --enable-fpm
  make && make install
</code></pre>
默认安装的路径是<em>/usr/local/php</em>，此外，在编译之前不知道具体的参数名的话可以输入 <em>./configure --help</em>命令进行查询，常用的参数有开启mysqlnd，gd，mcrypt之类的模块，按需添加即可。

安装完成后，在Nginx配置文件<em>/your/nginx/conf/path/nginx.conf</em>中与FastCGI相关的location中修改如下：
<pre><code class="highlighter">
  location ~ \.php$ {
      root           html;
      fastcgi_pass   127.0.0.1:9000;
      fastcgi_index  index.php;
      fastcgi_param  SCRIPT_FILENAME  /$document_root$fastcgi_script_name;
      include        fastcgi_params;
    }</code></pre>
至此，PHP已经编译完成且开启PHP-FPM，并设置Nginx将.php的文件交给PHP处理，使用如下命令启用PHP-FPM即可以开始工作了：
<pre><code class="highlighter">
  /usr/local/php/sbin/php-fpm -c /usr/local/php/etc/php.ini --pid /var/run/php-fpm.pid --fpm-config=/usr/local/php/etc/php-fpm.conf -D</code></pre>
这里给出相关参数的解释

* **-c** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;指定配置文件，在PHP源文件中可以找到production版和development版的ini文件，复制一份到上述目录中
* **--pid** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;指定程序运行的pid文件位置，可以通过该文件对进程进行控制
* **--fpm-config** 指定 PHP-FPM 使用其特有的配置文件
* **-D** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使程序在后台运行

此外，从PHP 5.4.0 起，安装PHP后我们可以使用内置的服务器进行一些本地开发工作，具体使用操作如下：
<pre><code class="highlighter">
  cd /your/files/path
  /usr/local/bin/php -S ip:port  // /usr/local/bin/php 是默认php编译安装路径
</code></pre>
输入命令之后就可以在浏览器中输入 ip:port 访问路径中的文件了，此外还可以通过 **-t** 参数指定根目录。

##使用 PECL
<a href="http://pecl.php.net" target="_blank">PECL</a>(PHP Extension Community Library)是一个PHP扩展库，简化了我们安装扩展库的工作。

**手动编译**扩展的过程一般是在PECL官网搜索并下载扩展库文件后，用phpize工具进行编译安装。以redis扩展为例，过程如下：

  1. 在官网搜索后得到redis扩展源码的下载主页是 **http://pecl.php.net/package/redis**
  2. 下载并解压源文件，这里是[redis-2.2.7.tgz](http://pecl.php.net/get/redis-2.2.7.tgz)
  3. 用命令解压并进入目录 **tar zxvf redis-2.2.7.tgz && cd redis-2.2.7**
  4. 使用PHP安装二进制目录下的 phpize 生成 configure 等编译所需文件 **/usr/local/php/bin/phpize**
  5. 配置参数 **./configure --with-php-config=/usr/local/php/bin/php-config**
  6. 编译并安装 **make && make install**

出现类似如下提示表明编译安装成功，提示**redis.so**文件生成在扩展目录中，扩展目录路径可以在**phpinfo()**函数中或者执行 **/usr/local/php/bin/php-config** 查看 **extension_dir**，最后在**php.ini** 添加上**extension=redis.so**即可。
<pre><code class="highlighter">
    Build complete.

    Don't forget to run 'make test'.
    
    Installing shared extensions:     /usr/local/php/lib/php/extensions/no-debug-non-zts-20100525/</code></pre>

**使用PECL**安装扩展则简单很多，依然以redis为例：
  
  1. 执行命令 **/usr/local/php/bin/pecl install redis**会自动下载安装编译扩展
  2. 出现如下提示后和手动编译中修改php.ini的操作一样添加**redis.so**
  <pre><code class="highlighter">
    Build process completed successfully
    Installing '/usr/local/php/lib/php/extensions/no-debug-non-zts-20100525/redis.so'
    install ok: channel://pecl.php.net/redis-2.2.7
    configuration option "php_ini" is not set to php.ini location
    You should add "extension=redis.so" to php.ini
  </code></pre>

既然提到了Redis扩展，也看到PECL的Redis扩展<a href="http://pecl.php.net/package/redis/2.2.7/windows" target="_blank">主页</a>上提供了Windows版的dll文件下载，顺便介绍一下Windows下PHP Redis扩展的安装方法。在扩展主页中根据自己PHP版本，**Zend Extension Build**信息，以及32位还是64位的信息下载相应的Redis的动态链接库即.dll文件，此外，还需要下载**php_igbinary.dll**扩展，官网提供了Windows下PECL的<a href="http://windows.php.net/downloads/pecl/releases/" target="_blank">下载地址</a>，在这里搜索igbinary下载对应的版本(上面提到的php_redis.dll也可以在这里下载)，将这两个文件放到PHP扩展目录中，并在php.ini中添加**extension=php_igbinary.dll**和**extension=php_redis.dll**这两句，重启即可。更多的参考信息在<a href="https://github.com/phpredis/phpredis/issues/213#issuecomment-11361242" target="_blank">这里</a>。

[官网PECL文档](http://php.net/manual/zh/install.pecl.php)还有更多有关下载和安装PECL的方法，在这里不做搬运工作了，官网写得很明白，官方的文档是我们学习的第一手资料，也常常被人们忽略。